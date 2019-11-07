---
title: Servicios en tiempo de diseño-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
uid: core/miscellaneous/cli/services
ms.openlocfilehash: 57294ab41e7c251b1dafae9d573aa98676c5d939
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655832"
---
# <a name="design-time-services"></a>Servicios en tiempo de diseño

Algunos servicios usados por las herramientas solo se usan en tiempo de diseño. Estos servicios se administran de forma independiente de los servicios en tiempo de ejecución de EF Core para evitar que se implementen con la aplicación. Para invalidar uno de estos servicios (por ejemplo, el servicio para generar archivos de migración), agregue una implementación de `IDesignTimeServices` al proyecto de inicio.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/DesignTimeServices.cs)]
