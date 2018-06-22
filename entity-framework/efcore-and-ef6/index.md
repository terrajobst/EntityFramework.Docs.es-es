---
title: Comparar EF Core y EF6
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: 4609ecbc9e24d8a359694d256523c64141b5ff62
ms.sourcegitcommit: 2ef0a4a90b01edd22b9206f8729b8de459ef8cab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2018
ms.locfileid: "30002761"
---
# <a name="compare-ef-core--ef6"></a>Comparar EF Core y EF6

Hay dos versiones distintas de Entity Framework: Entity Framework Core y Entity Framework 6.

## <a name="entity-framework-6"></a>Entity Framework 6

Entity Framework 6 (EF6) es una tecnología de acceso a datos probada con muchos años de características y estabilización. Se publicó por primera vez en 2008, como parte de .NET Framework 3.5 SP1 y Visual Studio 2008 SP1. A partir de la versión EF4.1, se ha distribuido como el [paquete NuGet EntityFramework](https://www.nuget.org/packages/EntityFramework/), actualmente uno de los paquetes más populares en NuGet.org.

EF6 sigue siendo un producto que recibe soporte técnico para el que seguirá habiendo correcciones de errores y pequeñas mejoras durante algún tiempo.

## <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework Core (EF Core) es una versión ligera, extensible y multiplataforma de Entity Framework. EF Core presenta numerosas mejoras y nuevas características en comparación con EF6. Al mismo tiempo, EF Core es un nuevo código base y no está tan maduro como EF6.

EF Core mantiene la experiencia del desarrollador de EF6 y, además, la mayoría de las API de nivel superior siguen siendo las mismas, por lo que EF Core resultará muy familiar a los usuarios que hayan usado EF6. Al mismo tiempo, EF Core se basa en un conjunto completamente nuevo de componentes principales. Esto significa que EF Core no hereda automáticamente todas las características de EF6. Algunas de estas características aparecerán en versiones futuras, mientras que otras de uso menos frecuente no se implementarán en EF Core.

El núcleo nuevo, que es extensible y ligero, también nos ha permitido agregar algunas características a EF Core que no se implementarán en EF6, como las claves alternativas, las actualizaciones por lotes y la evaluación mixta de cliente/base de datos en consultas LINQ.
