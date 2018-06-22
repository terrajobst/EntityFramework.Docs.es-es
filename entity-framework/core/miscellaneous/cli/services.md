---
title: Servicios en tiempo de diseño - Core EF
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
ms.technology: entity-framework-core
ms.openlocfilehash: f9c8208a59bfcefeaab01ea69e65fe809a0b3d89
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2017
ms.locfileid: "26053695"
---
<a name="design-time-services"></a>Servicios en tiempo de diseño
====================
Algunos servicios utilizados por las herramientas solo se usan en tiempo de diseño. Estos servicios se administran por separado de los servicios en tiempo de ejecución del núcleo de EF para impedir que se implementan con la aplicación. Para reemplazar uno de estos servicios (por ejemplo, el servicio para generar archivos de migración), agregue una implementación de `IDesignTimeServices` a su proyecto de inicio.

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
