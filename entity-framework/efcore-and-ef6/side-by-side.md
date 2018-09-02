---
title: 'EF6 y EF Core: uso en la misma aplicación'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: 6f95c02f4f24746605794832b1e26744fc554580
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995714"
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a>Uso de EF Core y EF6 en la misma aplicación

Es posible usar EF Core y EF6 en la misma biblioteca o aplicación de .NET Framework mediante la instalación de ambos paquetes NuGet.

Algunos tipos tienen los mismos nombres en EF Core y EF6 y solo difieren en el espacio de nombres, lo que puede complicar el uso de EF Core y EF6 en el mismo archivo de código. La ambigüedad se puede eliminar fácilmente con directivas de alias de espacios de nombres. Por ejemplo:

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

Si traslada una aplicación existente que tiene varios modelos de EF, puede elegir trasladar de manera selectiva algunos de ellos a EF Core y seguir usando EF6 para los demás.
