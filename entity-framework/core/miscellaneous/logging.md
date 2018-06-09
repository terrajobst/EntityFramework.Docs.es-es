---
title: Registro - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
ms.technology: entity-framework-core
uid: core/miscellaneous/logging
ms.openlocfilehash: 60d76bf3360eb47cdd9836494c1f135d1005a215
ms.sourcegitcommit: 3adf1267be92effc3c9daa893906a7f36834204f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/08/2018
ms.locfileid: "35232141"
---
# <a name="logging"></a><span data-ttu-id="9d023-102">Registro</span><span class="sxs-lookup"><span data-stu-id="9d023-102">Logging</span></span>

> [!TIP]  
> <span data-ttu-id="9d023-103">Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) de este artículo en GitHub.</span><span class="sxs-lookup"><span data-stu-id="9d023-103">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) on GitHub.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="9d023-104">Aplicaciones de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9d023-104">ASP.NET Core applications</span></span>

<span data-ttu-id="9d023-105">Núcleo EF se integra automáticamente con los mecanismos de registro de ASP.NET Core cada vez que `AddDbContext` o `AddDbContextPool` se utiliza.</span><span class="sxs-lookup"><span data-stu-id="9d023-105">EF Core integrates automatically with the logging mechanisms of ASP.NET Core whenever `AddDbContext` or `AddDbContextPool` is used.</span></span> <span data-ttu-id="9d023-106">Por lo tanto, cuando se usa ASP.NET Core, el registro debe configurarse como se describe en el [documentación principal de ASP.NET](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="9d023-106">Therefore, when using ASP.NET Core, logging should be configured as described in the [ASP.NET Core documentation](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

## <a name="other-applications"></a><span data-ttu-id="9d023-107">Otras aplicaciones</span><span class="sxs-lookup"><span data-stu-id="9d023-107">Other applications</span></span>

<span data-ttu-id="9d023-108">Núcleo EF registro actualmente requiere un ILoggerFactory que a su vez se configura con uno o más ILoggerProvider.</span><span class="sxs-lookup"><span data-stu-id="9d023-108">EF Core logging currently requires an ILoggerFactory which is itself configured with one or more ILoggerProvider.</span></span> <span data-ttu-id="9d023-109">Proveedores comunes se incluyen en los siguientes paquetes:</span><span class="sxs-lookup"><span data-stu-id="9d023-109">Common providers are shipped in the following packages:</span></span>

* <span data-ttu-id="9d023-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): un registrador de consola sencilla.</span><span class="sxs-lookup"><span data-stu-id="9d023-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): A simple console logger.</span></span>
* <span data-ttu-id="9d023-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): servicios de aplicaciones de Azure es compatible con 'Registros de diagnóstico' y 'Flujo de registro' características.</span><span class="sxs-lookup"><span data-stu-id="9d023-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supports Azure App Services 'Diagnostics logs' and 'Log stream' features.</span></span>
* <span data-ttu-id="9d023-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): registros de un monitor de depuración mediante System.Diagnostics.Debug.WriteLine().</span><span class="sxs-lookup"><span data-stu-id="9d023-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs to a debugger monitor using System.Diagnostics.Debug.WriteLine().</span></span>
* <span data-ttu-id="9d023-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): registros en el registro de eventos de Windows.</span><span class="sxs-lookup"><span data-stu-id="9d023-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs to Windows Event Log.</span></span>
* <span data-ttu-id="9d023-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): admite EventSource/EventListener.</span><span class="sxs-lookup"><span data-stu-id="9d023-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supports EventSource/EventListener.</span></span>
* <span data-ttu-id="9d023-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): registros de un agente de escucha de seguimiento mediante System.Diagnostics.TraceSource.TraceEvent().</span><span class="sxs-lookup"><span data-stu-id="9d023-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs to a trace listener using System.Diagnostics.TraceSource.TraceEvent().</span></span>

<span data-ttu-id="9d023-116">Después de instalar los paquetes adecuados, la aplicación debe crear una instancia singleton/global de un LoggerFactory.</span><span class="sxs-lookup"><span data-stu-id="9d023-116">After installing the appropriate package(s), the application should create a singleton/global instance of a LoggerFactory.</span></span> <span data-ttu-id="9d023-117">Por ejemplo, mediante el registrador de consola:</span><span class="sxs-lookup"><span data-stu-id="9d023-117">For example, using the console logger:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

<span data-ttu-id="9d023-118">Esta instancia de singleton o de forma global, a continuación, debe registrarse con EF principales en el `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="9d023-118">This singleton/global instance should then be registered with EF Core on the `DbContextOptionsBuilder`.</span></span> <span data-ttu-id="9d023-119">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="9d023-119">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> <span data-ttu-id="9d023-120">Es muy importante que las aplicaciones no crear una nueva instancia de ILoggerFactory para cada instancia de contexto.</span><span class="sxs-lookup"><span data-stu-id="9d023-120">It is very important that applications do not create a new ILoggerFactory instance for each context instance.</span></span> <span data-ttu-id="9d023-121">Si lo hace, se producirá una pérdida de memoria y un bajo rendimiento.</span><span class="sxs-lookup"><span data-stu-id="9d023-121">Doing so will result in a memory leak and poor performance.</span></span>

## <a name="filtering-what-is-logged"></a><span data-ttu-id="9d023-122">Lo que se registra el filtrado</span><span class="sxs-lookup"><span data-stu-id="9d023-122">Filtering what is logged</span></span>

<span data-ttu-id="9d023-123">Es la manera más fácil de filtrar lo que se registra configurarlo al registrar el ILoggerProvider.</span><span class="sxs-lookup"><span data-stu-id="9d023-123">The easiest way to filter what is logged is to configure it when registering the ILoggerProvider.</span></span> <span data-ttu-id="9d023-124">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="9d023-124">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

<span data-ttu-id="9d023-125">En este ejemplo, el registro se filtra para devolver sólo los mensajes:</span><span class="sxs-lookup"><span data-stu-id="9d023-125">In this example, the log is filtered to return only messages:</span></span>
 * <span data-ttu-id="9d023-126">en la categoría 'Microsoft.EntityFrameworkCore.Database.Command'</span><span class="sxs-lookup"><span data-stu-id="9d023-126">in the 'Microsoft.EntityFrameworkCore.Database.Command' category</span></span>
 * <span data-ttu-id="9d023-127">en el nivel de «Datos»</span><span class="sxs-lookup"><span data-stu-id="9d023-127">at the 'Information' level</span></span>

<span data-ttu-id="9d023-128">En el núcleo de EF, categorías de registrador se definen en la `DbLoggerCategory` clase para que sea fácil de encontrar categorías, pero estos resolver en cadenas simples.</span><span class="sxs-lookup"><span data-stu-id="9d023-128">For EF Core, logger categories are defined in the `DbLoggerCategory` class to make it easy to find categories, but these resolve to simple strings.</span></span>

<span data-ttu-id="9d023-129">Obtener más detalles sobre la infraestructura subyacente de registro pueden encontrarse en el [documentación de registro de ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="9d023-129">More details on the underlying logging infrastructure can be found in the [ASP.NET Core logging documentation](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>
