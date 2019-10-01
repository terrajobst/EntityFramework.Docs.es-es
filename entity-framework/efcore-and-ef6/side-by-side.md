---
title: 'EF6 y EF Core: uso en la misma aplicación'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: 8bf9f51c0e5c4b1b3adf4a6a9a894689dc13d2d9
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149297"
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a>Uso de EF Core y EF6 en la misma aplicación

Es posible usar EF Core y EF6 en la misma biblioteca o aplicación al instalar ambos paquetes NuGet.

Algunos tipos tienen los mismos nombres en EF Core y EF6 y solo difieren en el espacio de nombres, lo que puede complicar el uso de EF Core y EF6 en el mismo archivo de código. La ambigüedad se puede eliminar fácilmente con directivas de alias de espacios de nombres. Por ejemplo:

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

Si traslada una aplicación existente que tiene varios modelos de EF, puede elegir trasladar de manera selectiva algunos de ellos a EF Core y seguir usando EF6 para los demás.
