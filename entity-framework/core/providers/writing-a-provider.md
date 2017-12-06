---
title: Escribir a un proveedor de base de datos - Core EF
author: anmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
ms.technology: entity-framework-core
uid: core/providers/writing-a-provider
ms.openlocfilehash: 9d6d3748a9097b3b8eeee2a8a516c53f3b2afa58
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
---
# <a name="writing-a-database-provider"></a><span data-ttu-id="97319-102">Escribir un proveedor de base de datos</span><span class="sxs-lookup"><span data-stu-id="97319-102">Writing a Database Provider</span></span>

<span data-ttu-id="97319-103">Para obtener información sobre cómo escribir un proveedor de base de datos de Entity Framework Core, vea [por lo que desea escribir un proveedor de EF Core](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) por [Arthur Vickers](https://github.com/ajcvickers).</span><span class="sxs-lookup"><span data-stu-id="97319-103">For information about writing an Entity Framework Core database provider, see [So you want to write an EF Core provider](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) by [Arthur Vickers](https://github.com/ajcvickers).</span></span>

<span data-ttu-id="97319-104">La base de código de núcleo de EF es código abierto y contiene varios proveedores de base de datos que pueden usarse como referencia.</span><span class="sxs-lookup"><span data-stu-id="97319-104">The EF Core code base is open source and contains several database providers that can be used as a reference.</span></span> <span data-ttu-id="97319-105">Puede encontrar el código fuente en https://github.com/aspnet/EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="97319-105">You can find the source code at https://github.com/aspnet/EntityFramework.</span></span>

## <a name="the-providers-beware-label"></a><span data-ttu-id="97319-106">La etiqueta de proveedores beware</span><span class="sxs-lookup"><span data-stu-id="97319-106">The providers-beware label</span></span>

<span data-ttu-id="97319-107">Una vez que comenzar a trabajar en un proveedor, espere a que el [ `providers-beware` ](https://github.com/aspnet/EntityFramework/labels/providers-beware) etiqueta en nuestros GitHub problemas y solicitudes de extracción.</span><span class="sxs-lookup"><span data-stu-id="97319-107">Once you begin work on a provider, watch for the [`providers-beware`](https://github.com/aspnet/EntityFramework/labels/providers-beware) label on our GitHub issues and pull requests.</span></span> <span data-ttu-id="97319-108">Esta etiqueta se usa para identificar los cambios que puedan afectar a los escritores de proveedores.</span><span class="sxs-lookup"><span data-stu-id="97319-108">We use this label to identify changes that may impact provider writers.</span></span>

## <a name="suggested-naming-of-third-party-providers"></a><span data-ttu-id="97319-109">Sugiere la nomenclatura de los proveedores de terceros</span><span class="sxs-lookup"><span data-stu-id="97319-109">Suggested naming of third party providers</span></span>

<span data-ttu-id="97319-110">Se recomienda utilizar la siguiente nomenclatura para los paquetes de NuGet.</span><span class="sxs-lookup"><span data-stu-id="97319-110">We suggest using the following naming for NuGet packages.</span></span> <span data-ttu-id="97319-111">Esto es coherente con los nombres de los paquetes entregados por el equipo de EF Core.</span><span class="sxs-lookup"><span data-stu-id="97319-111">This is consistent with the names of packages delivered by the EF Core team.</span></span>

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

<span data-ttu-id="97319-112">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="97319-112">For example:</span></span>
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
