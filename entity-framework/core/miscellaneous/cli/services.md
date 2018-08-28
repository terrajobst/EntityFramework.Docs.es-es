---
title: Servicios de tiempo de diseño - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
ms.openlocfilehash: e1cacdd4f40f9c395d8c88a91df4a92ef27001a8
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997536"
---
<a name="design-time-services"></a><span data-ttu-id="05757-102">Servicios en tiempo de diseño</span><span class="sxs-lookup"><span data-stu-id="05757-102">Design-time services</span></span>
====================
<span data-ttu-id="05757-103">Algunos servicios que usan las herramientas solo se usan en tiempo de diseño.</span><span class="sxs-lookup"><span data-stu-id="05757-103">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="05757-104">Estos servicios se administran por separado de los servicios de tiempo de ejecución de EF Core para impedir que se implementarán con la aplicación.</span><span class="sxs-lookup"><span data-stu-id="05757-104">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="05757-105">Para reemplazar uno de estos servicios (por ejemplo, el servicio para generar los archivos de migración), agregue una implementación de `IDesignTimeServices` a su proyecto de inicio.</span><span class="sxs-lookup"><span data-stu-id="05757-105">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
