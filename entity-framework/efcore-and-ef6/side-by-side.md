---
title: "EF6 y EF Core: uso en la misma aplicación"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: f6eb4bf7d99fbc61f8ffbd0dc7c6c17789395303
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a><span data-ttu-id="58a34-102">Uso de EF Core y EF6 en la misma aplicación</span><span class="sxs-lookup"><span data-stu-id="58a34-102">Using EF Core and EF6 in the Same Application</span></span>

<span data-ttu-id="58a34-103">Es posible usar EF Core y EF6 en la misma biblioteca o aplicación de .NET Framework mediante la instalación de ambos paquetes NuGet.</span><span class="sxs-lookup"><span data-stu-id="58a34-103">It is possible to use EF Core and EF6 in the same .NET Framework application or library by installing both NuGet packages.</span></span> 

<span data-ttu-id="58a34-104">Algunos tipos tienen los mismos nombres en EF Core y EF6 y solo difieren en el espacio de nombres, lo que puede complicar el uso de EF Core y EF6 en el mismo archivo de código.</span><span class="sxs-lookup"><span data-stu-id="58a34-104">Some types have the same names in EF Core and EF6 and differ only by namespace, which may complicate using both EF Core and EF6 in the same code file.</span></span> <span data-ttu-id="58a34-105">La ambigüedad se puede eliminar fácilmente con directivas de alias de espacios de nombres, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="58a34-105">The ambiguity can be easily removed using namespace alias directives, e.g.:</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
using EF6 = System.Data.Entity; // e.g. EF6.DbContext
```

<span data-ttu-id="58a34-106">Si traslada una aplicación existente que tiene varios modelos de EF, puede elegir trasladar de manera selectiva algunos de ellos a EF Core y seguir usando EF6 para los demás.</span><span class="sxs-lookup"><span data-stu-id="58a34-106">If you are porting an existing application that has multiple EF models, you can choose to selectively port some of them to EF Core, and continue using EF6 for the others.</span></span>
