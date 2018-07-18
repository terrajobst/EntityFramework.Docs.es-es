---
title: 'EF6 y EF Core: uso en la misma aplicación'
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: ead251c5454473738c2f2bfdac6557aa3e1c5591
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949083"
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a><span data-ttu-id="40ef3-102">Uso de EF Core y EF6 en la misma aplicación</span><span class="sxs-lookup"><span data-stu-id="40ef3-102">Using EF Core and EF6 in the Same Application</span></span>

<span data-ttu-id="40ef3-103">Es posible usar EF Core y EF6 en la misma biblioteca o aplicación de .NET Framework mediante la instalación de ambos paquetes NuGet.</span><span class="sxs-lookup"><span data-stu-id="40ef3-103">It is possible to use EF Core and EF6 in the same .NET Framework application or library by installing both NuGet packages.</span></span>

<span data-ttu-id="40ef3-104">Algunos tipos tienen los mismos nombres en EF Core y EF6 y solo difieren en el espacio de nombres, lo que puede complicar el uso de EF Core y EF6 en el mismo archivo de código.</span><span class="sxs-lookup"><span data-stu-id="40ef3-104">Some types have the same names in EF Core and EF6 and differ only by namespace, which may complicate using both EF Core and EF6 in the same code file.</span></span> <span data-ttu-id="40ef3-105">La ambigüedad se puede eliminar fácilmente con directivas de alias de espacios de nombres.</span><span class="sxs-lookup"><span data-stu-id="40ef3-105">The ambiguity can be easily removed using namespace alias directives.</span></span> <span data-ttu-id="40ef3-106">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="40ef3-106">For example:</span></span>

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

<span data-ttu-id="40ef3-107">Si traslada una aplicación existente que tiene varios modelos de EF, puede elegir trasladar de manera selectiva algunos de ellos a EF Core y seguir usando EF6 para los demás.</span><span class="sxs-lookup"><span data-stu-id="40ef3-107">If you are porting an existing application that has multiple EF models, you can choose to selectively port some of them to EF Core, and continue using EF6 for the others.</span></span>
