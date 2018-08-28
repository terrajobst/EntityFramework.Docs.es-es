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
<a name="design-time-services"></a>Servicios en tiempo de diseño
====================
Algunos servicios que usan las herramientas solo se usan en tiempo de diseño. Estos servicios se administran por separado de los servicios de tiempo de ejecución de EF Core para impedir que se implementarán con la aplicación. Para reemplazar uno de estos servicios (por ejemplo, el servicio para generar los archivos de migración), agregue una implementación de `IDesignTimeServices` a su proyecto de inicio.

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
