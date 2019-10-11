---
title: Información general de Entity Framework 6 - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 8ae74d63-6bad-4686-b325-bbf9d68f3743
uid: ef6/index
ms.openlocfilehash: 9561a7c4b645896cb4e248cb094c6954ed4bcdf1
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181423"
---
# <a name="entity-framework-6"></a><span data-ttu-id="b5a4e-102">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="b5a4e-102">Entity Framework 6</span></span>
<span data-ttu-id="b5a4e-103">Entity Framework 6 (EF6) es un asignador relacional de objetos (O/RM) probado para .NET con muchos años de desarrollo de características y estabilización.</span><span class="sxs-lookup"><span data-stu-id="b5a4e-103">Entity Framework 6 (EF6) is a tried and tested object-relational mapper (O/RM) for .NET with many years of feature development and stabilization.</span></span>

<span data-ttu-id="b5a4e-104">Como O/RM, EF6 reduce la discordancia de impedancia entre los mundos relacionales y orientados a objetos, lo que permite a los desarrolladores escribir aplicaciones que interactúan con datos almacenados en bases de datos relacionales con objetos .NET fuertemente tipados que representan el dominio de la aplicación, y eliminar la necesidad de una gran parte del código de "mecánica" de acceso de datos que normalmente deben escribir.</span><span class="sxs-lookup"><span data-stu-id="b5a4e-104">As an O/RM, EF6 reduces the impedance mismatch between the relational and object-oriented worlds, enabling developers to write applications that interact with data stored in relational databases using strongly-typed .NET objects that represent the application's domain, and eliminating the need for a large portion of the data access "plumbing" code that they usually need to write.</span></span>

<span data-ttu-id="b5a4e-105">EF6 implementa muchas características de O/RM populares:</span><span class="sxs-lookup"><span data-stu-id="b5a4e-105">EF6 implements many popular O/RM features:</span></span>
- <span data-ttu-id="b5a4e-106">Asignación de clases de entidad [POCO](~/ef6/resources/glossary.md#poco) que no dependen de ningún tipo de EF</span><span class="sxs-lookup"><span data-stu-id="b5a4e-106">Mapping of [POCO](~/ef6/resources/glossary.md#poco) entity classes which do not depend on any EF types</span></span>
- <span data-ttu-id="b5a4e-107">Seguimiento de cambios automático</span><span class="sxs-lookup"><span data-stu-id="b5a4e-107">Automatic change tracking</span></span>
- <span data-ttu-id="b5a4e-108">Resolución de identidad y unidad de trabajo</span><span class="sxs-lookup"><span data-stu-id="b5a4e-108">Identity resolution and Unit of Work</span></span>
- <span data-ttu-id="b5a4e-109">Carga diligente, diferida y explícita</span><span class="sxs-lookup"><span data-stu-id="b5a4e-109">Eager, lazy and explicit loading</span></span>
- <span data-ttu-id="b5a4e-110">Traducción de consultas fuertemente tipadas con LINQ (Language Integrated Query)</span><span class="sxs-lookup"><span data-stu-id="b5a4e-110">Translation of strongly-typed queries using LINQ (Language INtegrated Query)</span></span>
- <span data-ttu-id="b5a4e-111">Capacidades de asignación enriquecidas que incluyen compatibilidad con:</span><span class="sxs-lookup"><span data-stu-id="b5a4e-111">Rich mapping capabilities, including support for:</span></span>
  - <span data-ttu-id="b5a4e-112">Relaciones de uno a uno, de uno a varios y entre varios</span><span class="sxs-lookup"><span data-stu-id="b5a4e-112">One-to-one, one-to-many and many-to-many relationships</span></span>
  - <span data-ttu-id="b5a4e-113">Herencia (tabla por jerarquía, tabla por tipo y tabla por clase concreta)</span><span class="sxs-lookup"><span data-stu-id="b5a4e-113">Inheritance (table per hierarchy, table per type and table per concrete class)</span></span>
  - <span data-ttu-id="b5a4e-114">Tipos complejos</span><span class="sxs-lookup"><span data-stu-id="b5a4e-114">Complex types</span></span>
  - <span data-ttu-id="b5a4e-115">Procedimientos almacenados</span><span class="sxs-lookup"><span data-stu-id="b5a4e-115">Stored procedures</span></span>
- <span data-ttu-id="b5a4e-116">Un diseñador visual para crear modelos de entidad.</span><span class="sxs-lookup"><span data-stu-id="b5a4e-116">A visual designer to create entity models.</span></span>
- <span data-ttu-id="b5a4e-117">Una experiencia "Code First" para crear modelos de entidad mediante la escritura de código.</span><span class="sxs-lookup"><span data-stu-id="b5a4e-117">A "Code First" experience to create entity models by writing code.</span></span>
- <span data-ttu-id="b5a4e-118">Los modelos pueden generarse a partir de bases de datos existentes y luego editarse manualmente, o bien se pueden crear desde cero y luego usarse para generar nuevas bases de datos.</span><span class="sxs-lookup"><span data-stu-id="b5a4e-118">Models can either be generated from existing databases and then hand-edited, or they can be created from scratch and then used to generate new databases.</span></span>
- <span data-ttu-id="b5a4e-119">Integración con modelos de aplicación de .NET Framework, incluido ASP.NET, y mediante enlace de datos, con WPF y WinForms.</span><span class="sxs-lookup"><span data-stu-id="b5a4e-119">Integration with .NET Framework application models, including ASP.NET, and through databinding, with WPF and WinForms.</span></span>
- <span data-ttu-id="b5a4e-120">Conectividad de base de datos basada en ADO.NET y varios proveedores disponibles para conectarse a SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2, etc.</span><span class="sxs-lookup"><span data-stu-id="b5a4e-120">Database connectivity based on ADO.NET and numerous providers available to connect to SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2, etc.</span></span>

## <a name="should-i-use-ef6-or-ef-core"></a><span data-ttu-id="b5a4e-121">¿Debo usar EF6 o EF Core?</span><span class="sxs-lookup"><span data-stu-id="b5a4e-121">Should I use EF6 or EF Core?</span></span>

<span data-ttu-id="b5a4e-122">EF Core es una versión más moderna, ligera y extensible de Entity Framework que tiene capacidades y ventajas muy similares a EF6.</span><span class="sxs-lookup"><span data-stu-id="b5a4e-122">EF Core is a more modern, lightweight and extensible version of Entity Framework that has very similar capabilities and benefits to EF6.</span></span>
<span data-ttu-id="b5a4e-123">EF Core es una reescritura completa y contiene muchas características nuevas que no están disponibles en EF6, aunque todavía carece de algunas de las funcionalidades más avanzadas de asignación de EF6.</span><span class="sxs-lookup"><span data-stu-id="b5a4e-123">EF Core is a complete rewrite and contains many new features not available in EF6, although it also still lacks some of the most advanced mapping capabilities of EF6.</span></span>
<span data-ttu-id="b5a4e-124">Considere el uso de EF Core en las aplicaciones nuevas si el conjunto de características se ajusta a los requisitos.</span><span class="sxs-lookup"><span data-stu-id="b5a4e-124">Consider using EF Core in new applications if the feature set matches your requirements.</span></span>
<span data-ttu-id="b5a4e-125">En [Comparar EF Core y EF6](xref:efcore-and-ef6/index) se examina el proceso de elección más detalladamente.</span><span class="sxs-lookup"><span data-stu-id="b5a4e-125">[Compare EF Core & EF6](xref:efcore-and-ef6/index) examines this choice in greater detail.</span></span>

## <a name="get-startedef6get-startedmd"></a>[<span data-ttu-id="b5a4e-126">Primeros pasos</span><span class="sxs-lookup"><span data-stu-id="b5a4e-126">Get Started</span></span>](~/ef6/get-started.md)

<span data-ttu-id="b5a4e-127">Agregue el paquete NuGet de EntityFramework al proyecto o instale Entity Framework Tools para Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b5a4e-127">Add the EntityFramework NuGet package to your project or install the Entity Framework Tools for Visual Studio.</span></span> <span data-ttu-id="b5a4e-128">Luego, vea vídeos, lea tutoriales y consulte documentación avanzada, que le ayudarán a sacar el máximo partido de EF6.</span><span class="sxs-lookup"><span data-stu-id="b5a4e-128">Then watch videos, read tutorials, and advanced documentation to help you make the most of EF6.</span></span>

## <a name="past-entity-framework-versions"></a><span data-ttu-id="b5a4e-129">Versiones anteriores de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="b5a4e-129">Past Entity Framework Versions</span></span>

<span data-ttu-id="b5a4e-130">Esta es la documentación de la versión más reciente de Entity Framework 6, aunque la mayor parte también se aplica a las versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="b5a4e-130">This is the documentation for the latest version of Entity Framework 6, although much of it also applies to past releases.</span></span>
<span data-ttu-id="b5a4e-131">Vea [Novedades](~/ef6/what-is-new/index.md) y [Versiones anteriores](~/ef6/what-is-new/past-releases.md) para obtener una lista completa de las versiones de EF y las características incluidas.</span><span class="sxs-lookup"><span data-stu-id="b5a4e-131">Check out [What's New](~/ef6/what-is-new/index.md) and [Past Releases](~/ef6/what-is-new/past-releases.md) for a complete list of EF releases and the features they introduced.</span></span>
