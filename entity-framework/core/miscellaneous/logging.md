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
# <a name="logging"></a><span data-ttu-id="96742-102">Registro</span><span class="sxs-lookup"><span data-stu-id="96742-102">Logging</span></span>

> [!TIP]  
> <span data-ttu-id="96742-103">Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) de este artículo en GitHub.</span><span class="sxs-lookup"><span data-stu-id="96742-103">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) on GitHub.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="96742-104">Aplicaciones ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="96742-104">ASP.NET Core applications</span></span>

<span data-ttu-id="96742-105">EF Core se integra automáticamente con los mecanismos de registro de ASP.NET Core siempre que `AddDbContext` o `AddDbContextPool` se utiliza.</span><span class="sxs-lookup"><span data-stu-id="96742-105">EF Core integrates automatically with the logging mechanisms of ASP.NET Core whenever `AddDbContext` or `AddDbContextPool` is used.</span></span> <span data-ttu-id="96742-106">Por lo tanto, cuando se usa ASP.NET Core, el registro debe configurarse como se describe en el [documentación de ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="96742-106">Therefore, when using ASP.NET Core, logging should be configured as described in the [ASP.NET Core documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

## <a name="other-applications"></a><span data-ttu-id="96742-107">Otras aplicaciones</span><span class="sxs-lookup"><span data-stu-id="96742-107">Other applications</span></span>

<span data-ttu-id="96742-108">EF Core actualmente registro requiere un elemento ILoggerFactory que se configura con uno o más ILoggerProvider.</span><span class="sxs-lookup"><span data-stu-id="96742-108">EF Core logging currently requires an ILoggerFactory which is itself configured with one or more ILoggerProvider.</span></span> <span data-ttu-id="96742-109">Proveedores comunes se incluyen en los siguientes paquetes:</span><span class="sxs-lookup"><span data-stu-id="96742-109">Common providers are shipped in the following packages:</span></span>

* <span data-ttu-id="96742-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): Un registrador de consola sencilla.</span><span class="sxs-lookup"><span data-stu-id="96742-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): A simple console logger.</span></span>
* <span data-ttu-id="96742-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Es compatible con Azure App Services 'Registros de diagnóstico' y 'Iniciar secuencia' características.</span><span class="sxs-lookup"><span data-stu-id="96742-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supports Azure App Services 'Diagnostics logs' and 'Log stream' features.</span></span>
* <span data-ttu-id="96742-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Registros para un monitor de depuración mediante System.Diagnostics.Debug.WriteLine().</span><span class="sxs-lookup"><span data-stu-id="96742-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs to a debugger monitor using System.Diagnostics.Debug.WriteLine().</span></span>
* <span data-ttu-id="96742-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Registros para el registro de eventos de Windows.</span><span class="sxs-lookup"><span data-stu-id="96742-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs to Windows Event Log.</span></span>
* <span data-ttu-id="96742-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Admite EventSource/EventListener.</span><span class="sxs-lookup"><span data-stu-id="96742-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supports EventSource/EventListener.</span></span>
* <span data-ttu-id="96742-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Registros para un agente de escucha de seguimiento mediante System.Diagnostics.TraceSource.TraceEvent().</span><span class="sxs-lookup"><span data-stu-id="96742-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs to a trace listener using System.Diagnostics.TraceSource.TraceEvent().</span></span>

> [!NOTE]
> <span data-ttu-id="96742-116">El siguiente código de ejemplo se usa un `ConsoleLoggerProvider` constructor que ha sido obsoleto en la versión 2.2.</span><span class="sxs-lookup"><span data-stu-id="96742-116">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2.</span></span> <span data-ttu-id="96742-117">Reemplazos adecuados para la API de registro obsoleta estará disponibles en la versión 3.0.</span><span class="sxs-lookup"><span data-stu-id="96742-117">Proper replacements for obsolete logging APIs will be available in version 3.0.</span></span> <span data-ttu-id="96742-118">Mientras tanto, es seguro pasar por alto y suprimir las advertencias.</span><span class="sxs-lookup"><span data-stu-id="96742-118">In the meantime, it is safe to ignore and suppress the warnings.</span></span>

<span data-ttu-id="96742-119">Después de instalar los paquetes adecuados, la aplicación debe crear una instancia de un LoggerFactory singleton o global.</span><span class="sxs-lookup"><span data-stu-id="96742-119">After installing the appropriate package(s), the application should create a singleton/global instance of a LoggerFactory.</span></span> <span data-ttu-id="96742-120">Por ejemplo, mediante el registrador de consola:</span><span class="sxs-lookup"><span data-stu-id="96742-120">For example, using the console logger:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

<span data-ttu-id="96742-121">Esta instancia de singleton o global, a continuación, se debe registrar con EF Core en el `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="96742-121">This singleton/global instance should then be registered with EF Core on the `DbContextOptionsBuilder`.</span></span> <span data-ttu-id="96742-122">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="96742-122">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> <span data-ttu-id="96742-123">Es muy importante que las aplicaciones no crean una nueva instancia de ILoggerFactory para cada instancia del contexto.</span><span class="sxs-lookup"><span data-stu-id="96742-123">It is very important that applications do not create a new ILoggerFactory instance for each context instance.</span></span> <span data-ttu-id="96742-124">Si lo hace, dará como resultado una pérdida de memoria y un rendimiento deficiente.</span><span class="sxs-lookup"><span data-stu-id="96742-124">Doing so will result in a memory leak and poor performance.</span></span>

## <a name="filtering-what-is-logged"></a><span data-ttu-id="96742-125">Lo que se registra el filtrado</span><span class="sxs-lookup"><span data-stu-id="96742-125">Filtering what is logged</span></span>

> [!NOTE]
> <span data-ttu-id="96742-126">El siguiente código de ejemplo se usa un `ConsoleLoggerProvider` constructor que ha sido obsoleto en la versión 2.2.</span><span class="sxs-lookup"><span data-stu-id="96742-126">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2.</span></span> <span data-ttu-id="96742-127">Reemplazos adecuados para la API de registro obsoleta estará disponibles en la versión 3.0.</span><span class="sxs-lookup"><span data-stu-id="96742-127">Proper replacements for obsolete logging APIs will be available in version 3.0.</span></span> <span data-ttu-id="96742-128">Mientras tanto, es seguro pasar por alto y suprimir las advertencias.</span><span class="sxs-lookup"><span data-stu-id="96742-128">In the meantime, it is safe to ignore and suppress the warnings.</span></span>

<span data-ttu-id="96742-129">Filtrar lo que se registra la manera más fácil es configurarlo al registrar el ILoggerProvider.</span><span class="sxs-lookup"><span data-stu-id="96742-129">The easiest way to filter what is logged is to configure it when registering the ILoggerProvider.</span></span> <span data-ttu-id="96742-130">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="96742-130">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

<span data-ttu-id="96742-131">En este ejemplo, el registro se puede filtrar para devolver sólo los mensajes:</span><span class="sxs-lookup"><span data-stu-id="96742-131">In this example, the log is filtered to return only messages:</span></span>
 * <span data-ttu-id="96742-132">en la categoría 'Microsoft.EntityFrameworkCore.Database.Command'</span><span class="sxs-lookup"><span data-stu-id="96742-132">in the 'Microsoft.EntityFrameworkCore.Database.Command' category</span></span>
 * <span data-ttu-id="96742-133">en el nivel "Información"</span><span class="sxs-lookup"><span data-stu-id="96742-133">at the 'Information' level</span></span>

<span data-ttu-id="96742-134">Para EF Core, se definen las categorías de registrador en el `DbLoggerCategory` clase para que sea más fácil encontrar las categorías, pero estos resolver como cadenas simples.</span><span class="sxs-lookup"><span data-stu-id="96742-134">For EF Core, logger categories are defined in the `DbLoggerCategory` class to make it easy to find categories, but these resolve to simple strings.</span></span>

<span data-ttu-id="96742-135">Encontrará más detalles sobre la infraestructura subyacente de registro en el [documentación de registro de ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="96742-135">More details on the underlying logging infrastructure can be found in the [ASP.NET Core logging documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>
