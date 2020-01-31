---
title: 'EF6 y EF Core: uso en la misma aplicación'
author: ajcvickers
ms.date: 01/23/2019
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: bcf0a26535c4ec880a9ac25478c987fb683f6d26
ms.sourcegitcommit: b3cf5d2e3cb170b9916795d1d8c88678269639b1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2020
ms.locfileid: "76888140"
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a><span data-ttu-id="36d45-102">Uso de EF Core y EF6 en la misma aplicación</span><span class="sxs-lookup"><span data-stu-id="36d45-102">Using EF Core and EF6 in the Same Application</span></span>

<span data-ttu-id="36d45-103">Es posible usar EF Core y EF6 en la misma biblioteca o aplicación al instalar ambos paquetes NuGet.</span><span class="sxs-lookup"><span data-stu-id="36d45-103">It is possible to use EF Core and EF6 in the same application or library by installing both NuGet packages.</span></span>

<span data-ttu-id="36d45-104">Algunos tipos tienen los mismos nombres en EF Core y EF6 y solo difieren en el espacio de nombres, lo que puede complicar el uso de EF Core y EF6 en el mismo archivo de código.</span><span class="sxs-lookup"><span data-stu-id="36d45-104">Some types have the same names in EF Core and EF6 and differ only by namespace, which may complicate using both EF Core and EF6 in the same code file.</span></span> <span data-ttu-id="36d45-105">La ambigüedad se puede eliminar fácilmente con directivas de alias de espacios de nombres.</span><span class="sxs-lookup"><span data-stu-id="36d45-105">The ambiguity can be easily removed using namespace alias directives.</span></span> <span data-ttu-id="36d45-106">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="36d45-106">For example:</span></span>

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

<span data-ttu-id="36d45-107">Si traslada una aplicación existente que tiene varios modelos de EF, puede elegir trasladar de manera selectiva algunos de ellos a EF Core y seguir usando EF6 para los demás.</span><span class="sxs-lookup"><span data-stu-id="36d45-107">If you are porting an existing application that has multiple EF models, you can choose to selectively port some of them to EF Core, and continue using EF6 for the others.</span></span>
