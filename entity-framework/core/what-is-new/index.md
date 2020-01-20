---
title: Versiones y planeamiento de EF Core
author: ajcvickers
ms.date: 01/14/2020
ms.assetid: C21F89EE-FB08-4ED9-A2A0-76CB7656E6E4
uid: core/what-is-new/index
ms.openlocfilehash: 8d74c24021fd62c5c5d944eaf3973b344fdb1e9c
ms.sourcegitcommit: f2a38c086291699422d8b28a72d9611d1b24ad0d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2020
ms.locfileid: "76124410"
---
# <a name="ef-core-releases-and-planning"></a>Versiones y planeamiento de EF Core

## <a name="stable-releases"></a>Versiones estables

| Release | Marco de trabajo de destino | Compatibilidad hasta | Vínculos
|:--------|------------------|-----------------|------
| [EF Core 3.1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/3.1.1) | .NET Standard 2.0 | 3 de diciembre de 2022 (LTS) | [Anuncio](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-3-1-and-entity-framework-6-4/)
| [EF Core 3.0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/3.0.1) | .NET Standard 2.1 | 3 de marzo de 2020 | [Anuncio](https://devblogs.microsoft.com/dotnet/announcing-ef-core-3-0-and-ef-6-3-general-availability/) / [Cambios importantes](ef-core-3.0/breaking-changes.md)
| ~~[EF Core 2.2](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/2.2.6)~~ | .NET Standard 2.0 | Expiró el 23 de diciembre de 2019 | [Anuncio](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-2/)
| [EF Core 2.1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/2.1.14) | .NET Standard 2.0 | 21 de agosto de 2021 (LTS) | [Anuncio](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-1/)
| ~~[EF Core 2.0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/2.0.3)~~ | .NET Standard 2.0 | Expiró el 1 de octubre de 2018 | [Anuncio](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-0/)
| ~~[EF Core 1.1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/1.1.6)~~ | .NET Standard 1.3 | Expiró el 27 de junio de 2019 | [Anuncio](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-1-1/)
| ~~[EF Core 1.0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/1.0.6)~~ | .NET Standard 1.3 | Expiró el 27 de junio de 2019 | [Anuncio](https://devblogs.microsoft.com/dotnet/entity-framework-core-1-0-0-available/)

Consulte las [plataformas compatibles](../platforms/index.md) para saber qué plataformas concretas se admiten en cada versión de EF Core.

Consulte la [Directiva de compatibilidad de .NET](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) para obtener información sobre la fecha de expiración de la compatibilidad y las versiones de compatibilidad a largo plazo (LTS).

## <a name="guidance-on-updating-to-new-releases"></a>Instrucciones para actualizar a nuevas versiones

* Las versiones admitidas se revisan por motivos de seguridad y para solucionar otros errores críticos. Use siempre el parche más reciente de una versión determinada. Por ejemplo, para EF Core 2.1, use 2.1.14.
* Las actualizaciones de la versión principal (por ejemplo, de EF Core 2 a EF Core 3) suelen tener cambios importantes. Se recomienda realizar pruebas exhaustivas al cambiar de una versión principal a otra. Use los vínculos de cambios importantes anteriores para obtener información sobre cómo abordar los cambios importantes.
* Las actualizaciones de versiones secundarias no suelen contener cambios importantes. No obstante, sigue siendo aconsejable realizar pruebas exhaustivas, ya que las nuevas características pueden introducir regresiones.

## <a name="ef-core-50"></a>EF Core 5.0

Las versiones de EF Core siguen la [programación de envío de .NET Core](https://github.com/dotnet/core/blob/master/roadmap.md). La siguiente versión estable planeada es **EF Core 5.0**, programada para noviembre de 2020.

Se ha creado un [plan de alto nivel para EF Core 5.0](ef-core-5.0/plan.md) siguiendo el [proceso de planeamiento de versiones](release-planning.md) documentado.

Sus comentarios sobre la planeación son importantes. La mejor manera de indicar la importancia de un problema es votar (pulgar) por ese problema en GitHub. Estos datos se introducen en el proceso de planeación de la próxima versión.

### <a name="get-it-now"></a>¡Obténgalo ahora!

Los paquetes de EF Core 5.0 están **disponibles ahora** como [compilaciones diarias](https://github.com/aspnet/AspNetCore/blob/master/docs/DailyBuilds.md). 

Usar las compilaciones diarias es una excelente manera de encontrar problemas y proporcionar comentarios lo antes posible. Cuanto antes recibamos esos comentarios, más probable será que puedan procesarse antes de la siguiente versión oficial. Dedicamos grandes esfuerzos a cuidar el estado de las compilaciones diarias, como demuestran las más de 55 000 pruebas por plataforma que llevamos a cabo para cada compilación.

Los paquetes de versión preliminar se enviarán a NuGet a lo largo del año.
