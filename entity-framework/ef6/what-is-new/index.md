---
title: 'Novedades: EF6'
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
ms.openlocfilehash: 568790d9c9bb7dd2213907bef8fa090710cd3ba0
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149119"
---
# <a name="whats-new-in-ef6"></a>Novedades de EF6

Se recomienda encarecidamente usar la versión más reciente de Entity Framework para asegurarse de obtener las últimas características y la mayor estabilidad.
Pero es posible que deba usar una versión anterior o que quiera experimentar con las nuevas mejoras de la última versión preliminar.
Para instalar versiones concretas de EF, vea [Get Entity Framework](~/ef6/fundamentals/install.md) (Obtener Entity Framework).

## <a name="ef-630"></a>EF 6.3.0

El runtime de EF 6.3.0 para NuGet se publicó en septiembre de 2019. El objetivo principal de esta versión era facilitar la migración de las aplicaciones existentes que usan EF 6 a .NET Core 3.0. La comunidad también ha contribuido con varias correcciones de errores y mejoras. Consulte los problemas cerrados en cada [hito](https://github.com/aspnet/EntityFramework6/milestones?state=closed) de la versión 6.3.0 para información detallada. Estos son algunos de los más importantes:

- Compatibilidad con .NET Core 3.0.
  - El paquete EntityFramework ahora tiene como destino .NET Standard 2.1 además de .NET Framework 4.x.
  - Se han vuelto a escribir los comandos de migración para ejecutarlos fuera de proceso y que trabajen con proyectos de estilo de SDK.
- Compatibilidad con HierarchyId de SQL Server
- Compatibilidad mejorada con PackageReference de Roslyn y NuGet
- Se agregó ef6.exe para habilitar, agregar, generar scripts y aplicar migraciones desde los ensamblados. Esto reemplaza a migrate.exe.

## <a name="past-releases"></a>Versiones anteriores

La página [Versiones anteriores](past-releases.md) contiene un archivo de todas las versiones anteriores de EF y de las características principales incluidas en cada versión.
