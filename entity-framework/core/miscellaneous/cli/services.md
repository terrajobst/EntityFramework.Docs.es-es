---
title: "Servicios en tiempo de diseño - Core EF"
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
ms.technology: entity-framework-core
ms.openlocfilehash: f9c8208a59bfcefeaab01ea69e65fe809a0b3d89
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2017
---
<a name="design-time-services"></a><span data-ttu-id="eac79-102">Servicios en tiempo de diseño</span><span class="sxs-lookup"><span data-stu-id="eac79-102">Design-time services</span></span>
====================
<span data-ttu-id="eac79-103">Algunos servicios utilizados por las herramientas solo se usan en tiempo de diseño.</span><span class="sxs-lookup"><span data-stu-id="eac79-103">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="eac79-104">Estos servicios se administran por separado de los servicios en tiempo de ejecución del núcleo de EF para impedir que se implementan con la aplicación.</span><span class="sxs-lookup"><span data-stu-id="eac79-104">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="eac79-105">Para reemplazar uno de estos servicios (por ejemplo, el servicio para generar archivos de migración), agregue una implementación de `IDesignTimeServices` a su proyecto de inicio.</span><span class="sxs-lookup"><span data-stu-id="eac79-105">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
