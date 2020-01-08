---
title: Herramientas y extensiones - EF Core
author: ErikEJ
ms.date: 12/17/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: bab725afffe1fbf9f8c0abeef58579ac9dc842d2
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502087"
---
# <a name="ef-core-tools--extensions"></a><span data-ttu-id="ee72d-102">Herramientas y extensiones de EF Core</span><span class="sxs-lookup"><span data-stu-id="ee72d-102">EF Core Tools & Extensions</span></span>

<span data-ttu-id="ee72d-103">Estas herramientas y extensiones proporcionan más funcionalidades para Entity Framework Core 2.1 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="ee72d-103">These tools and extensions provide additional functionality for Entity Framework Core 2.1 and later.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="ee72d-104">Las extensiones están compiladas a partir de una gran variedad de orígenes, por lo que su mantenimiento no está incluido en el proyecto Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="ee72d-104">Extensions are built by a variety of sources and aren't maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="ee72d-105">En lo que respecta a las extensiones de terceros y para asegurarse de que cumplan sus requisitos, no se olvide de evaluar la calidad, las licencias, la compatibilidad, el soporte técnico, etc.</span><span class="sxs-lookup"><span data-stu-id="ee72d-105">When considering a third party extension, be sure to evaluate its quality, licensing, compatibility, support, etc. to ensure it meets your requirements.</span></span> <span data-ttu-id="ee72d-106">En concreto, es posible que sea necesario actualizar una extensión compilada para una versión anterior de EF Core para que funcione con las versiones más recientes.</span><span class="sxs-lookup"><span data-stu-id="ee72d-106">In particular, an extension built for an older version of EF Core may need updating before it will work with the latest versions.</span></span>

## <a name="tools"></a><span data-ttu-id="ee72d-107">Herramientas</span><span class="sxs-lookup"><span data-stu-id="ee72d-107">Tools</span></span>

### <a name="llblgen-pro"></a><span data-ttu-id="ee72d-108">LLBLGen Pro</span><span class="sxs-lookup"><span data-stu-id="ee72d-108">LLBLGen Pro</span></span>

<span data-ttu-id="ee72d-109">LLBLGen Pro es una solución de modelado de entidad compatible con Entity Framework y Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="ee72d-109">LLBLGen Pro is an entity modeling solution with support for Entity Framework and Entity Framework Core.</span></span> <span data-ttu-id="ee72d-110">Permite definir fácilmente el modelo de entidad y asignarlo a la base de datos mediante Database First o Model First, de modo que pueda empezar a escribir consultas de inmediato.</span><span class="sxs-lookup"><span data-stu-id="ee72d-110">It lets you easily define your entity model and map it to your database, using database first or model first, so you can get started writing queries right away.</span></span> <span data-ttu-id="ee72d-111">Para EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="ee72d-111">For EF Core: 2.</span></span>

[<span data-ttu-id="ee72d-112">Sitio web</span><span class="sxs-lookup"><span data-stu-id="ee72d-112">Website</span></span>](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a><span data-ttu-id="ee72d-113">Devart Entity Developer</span><span class="sxs-lookup"><span data-stu-id="ee72d-113">Devart Entity Developer</span></span>

<span data-ttu-id="ee72d-114">Entity Developer es un potente diseñador ORM para ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access y LINQ to SQL.</span><span class="sxs-lookup"><span data-stu-id="ee72d-114">Entity Developer is a powerful ORM designer for ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access, and LINQ to SQL.</span></span> <span data-ttu-id="ee72d-115">Permite diseñar modelos EF Core de forma visual mediante los enfoques Database First y Model First, así como generar código C# y de Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="ee72d-115">It supports designing EF Core models visually, using model first or database first approaches, and C# or Visual Basic code generation.</span></span> <span data-ttu-id="ee72d-116">Para EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="ee72d-116">For EF Core: 2.</span></span>

[<span data-ttu-id="ee72d-117">Sitio web</span><span class="sxs-lookup"><span data-stu-id="ee72d-117">Website</span></span>](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a><span data-ttu-id="ee72d-118">EF Core Power Tools</span><span class="sxs-lookup"><span data-stu-id="ee72d-118">EF Core Power Tools</span></span>

<span data-ttu-id="ee72d-119">EF Core Power Tools es una extensión de Visual Studio que expone varias tareas de tiempo de diseño de EF Core en una interfaz de usuario sencilla.</span><span class="sxs-lookup"><span data-stu-id="ee72d-119">EF Core Power Tools is a Visual Studio extension that exposes various EF Core design-time tasks in a simple user interface.</span></span> <span data-ttu-id="ee72d-120">Incluye técnicas de ingeniería inversa de DbContext y clases de entidades a partir de bases de datos existentes y [DACPAC de SQL Server](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), así como la administración de la migración de bases de datos y la visualización de modelos.</span><span class="sxs-lookup"><span data-stu-id="ee72d-120">It includes reverse engineering of DbContext and entity classes from existing databases and [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), management of database migrations, and model visualizations.</span></span> <span data-ttu-id="ee72d-121">Para EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="ee72d-121">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="ee72d-122">Wiki de GitHub</span><span class="sxs-lookup"><span data-stu-id="ee72d-122">GitHub wiki</span></span>](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a><span data-ttu-id="ee72d-123">Editor visual de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="ee72d-123">Entity Framework Visual Editor</span></span>

<span data-ttu-id="ee72d-124">El Editor de objetos visuales de Entity Framework es una extensión de Visual Studio que agrega un diseñador ORM que permite generar objetos visuales de las clases EF 6 y EF Core.</span><span class="sxs-lookup"><span data-stu-id="ee72d-124">Entity Framework Visual Editor is a Visual Studio extension that adds an ORM designer for visual design of EF 6, and EF Core classes.</span></span> <span data-ttu-id="ee72d-125">El código se genera mediante plantillas T4, por lo que se puede personalizar.</span><span class="sxs-lookup"><span data-stu-id="ee72d-125">Code is generated using T4 templates so can be customized to suit any needs.</span></span> <span data-ttu-id="ee72d-126">Asimismo, admite enumeraciones y asociaciones de herencia, unidireccionales y bidireccionales, y permite asignar colores a las clases y agregar bloques de texto para explicar ciertas partes del diseño que puedan resultar difíciles de comprender.</span><span class="sxs-lookup"><span data-stu-id="ee72d-126">It supports inheritance, unidirectional and bidirectional associations, enumerations, and the ability to color-code your classes and add text blocks to explain potentially arcane parts of your design.</span></span> <span data-ttu-id="ee72d-127">Para EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="ee72d-127">For EF Core: 2.</span></span>

[<span data-ttu-id="ee72d-128">Marketplace</span><span class="sxs-lookup"><span data-stu-id="ee72d-128">Marketplace</span></span>](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a><span data-ttu-id="ee72d-129">CatFactory</span><span class="sxs-lookup"><span data-stu-id="ee72d-129">CatFactory</span></span>

<span data-ttu-id="ee72d-130">CatFactory es un motor de scaffolding para .NET Core que permite automatizar la generación de clases DbContext, entidades, opciones de configuración de la asignación y clases de repositorios de una base de datos SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ee72d-130">CatFactory is a scaffolding engine for .NET Core that can automate the generation of DbContext classes, entities, mapping configurations, and repository classes from a SQL Server database.</span></span> <span data-ttu-id="ee72d-131">Para EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="ee72d-131">For EF Core: 2.</span></span>

[<span data-ttu-id="ee72d-132">Repositorio de GitHub</span><span class="sxs-lookup"><span data-stu-id="ee72d-132">GitHub repository</span></span>](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a><span data-ttu-id="ee72d-133">LoreSoft's Entity Framework Core Generator</span><span class="sxs-lookup"><span data-stu-id="ee72d-133">LoreSoft's Entity Framework Core Generator</span></span>

<span data-ttu-id="ee72d-134">Entity Framework Core Generator (efg) es una herramienta de la CLI de .NET Core que permite generar modelos EF Core a partir de una base de datos existente. Es similar a `dotnet ef dbcontext scaffold`, pero también admite la [regeneración](https://efg.loresoft.com/en/latest/regeneration/) de código seguro mediante el reemplazo de la región o el análisis de los archivos de asignación.</span><span class="sxs-lookup"><span data-stu-id="ee72d-134">Entity Framework Core Generator (efg) is a .NET Core CLI tool that can generate EF Core models from an existing database, much like `dotnet ef dbcontext scaffold`, but it also supports safe code [regeneration](https://efg.loresoft.com/en/latest/regeneration/) via region replacement or by parsing mapping files.</span></span> <span data-ttu-id="ee72d-135">Asimismo, la herramienta permite generar modelos de vista, efectuar la validación y crear el código de los asignadores de objetos.</span><span class="sxs-lookup"><span data-stu-id="ee72d-135">This tool supports generating view models, validation, and object mapper code.</span></span> <span data-ttu-id="ee72d-136">Para EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="ee72d-136">For EF Core: 2.</span></span>

<span data-ttu-id="ee72d-137">[Tutorial](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentación](https://efg.loresoft.com/en/latest/)</span><span class="sxs-lookup"><span data-stu-id="ee72d-137">[Tutorial](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentation](https://efg.loresoft.com/en/latest/)</span></span>

## <a name="extensions"></a><span data-ttu-id="ee72d-138">Extensiones</span><span class="sxs-lookup"><span data-stu-id="ee72d-138">Extensions</span></span>

### <a name="microsoftentityframeworkcoreautohistory"></a><span data-ttu-id="ee72d-139">Microsoft.EntityFrameworkCore.AutoHistory</span><span class="sxs-lookup"><span data-stu-id="ee72d-139">Microsoft.EntityFrameworkCore.AutoHistory</span></span>

<span data-ttu-id="ee72d-140">Biblioteca de extensiones que permite registrar automáticamente los cambios en los datos realizados por EF Core e incluirlos en una tabla a modo de historial.</span><span class="sxs-lookup"><span data-stu-id="ee72d-140">A plugin library that enables automatically recording the data changes performed by EF Core into a history table.</span></span> <span data-ttu-id="ee72d-141">Para EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="ee72d-141">For EF Core: 2.</span></span>

[<span data-ttu-id="ee72d-142">Repositorio de GitHub</span><span class="sxs-lookup"><span data-stu-id="ee72d-142">GitHub repository</span></span>](https://github.com/Arch/AutoHistory/)

### <a name="efsecondlevelcachecore"></a><span data-ttu-id="ee72d-143">EFSecondLevelCache.Core</span><span class="sxs-lookup"><span data-stu-id="ee72d-143">EFSecondLevelCache.Core</span></span>

<span data-ttu-id="ee72d-144">Extensión que permite almacenar los resultados de las consultas EF Core en una caché de segundo nivel. De este modo, al volver a ejecutar las mismas cadenas, se pueden recuperar los datos directamente de la caché sin acceder a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="ee72d-144">An extension that enables storing the results of EF Core queries into a second-level cache, so that subsequent executions of the same queries can avoid accessing the database and retrieve the data directly from the cache.</span></span> <span data-ttu-id="ee72d-145">Para EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="ee72d-145">For EF Core: 2.</span></span>

[<span data-ttu-id="ee72d-146">Repositorio de GitHub</span><span class="sxs-lookup"><span data-stu-id="ee72d-146">GitHub repository</span></span>](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="geco"></a><span data-ttu-id="ee72d-147">Geco</span><span class="sxs-lookup"><span data-stu-id="ee72d-147">Geco</span></span>

<span data-ttu-id="ee72d-148">Geco (Generator Console) es un generador de código muy sencillo que se basa en un proyecto de consola. Para generar el código, se ejecuta en .NET Core y utiliza cadenas C# interpoladas.</span><span class="sxs-lookup"><span data-stu-id="ee72d-148">Geco (Generator Console) is a simple code generator based on a console project, that runs on .NET Core and uses C# interpolated strings for code generation.</span></span> <span data-ttu-id="ee72d-149">Geco incluye un generador de modelos de ingeniería inversa para EF Core que admite la pluralización, la singularización y las plantillas editables.</span><span class="sxs-lookup"><span data-stu-id="ee72d-149">Geco includes a reverse model generator for EF Core with support for pluralization, singularization, and editable templates.</span></span> <span data-ttu-id="ee72d-150">También proporciona un generador de scripts de datos semilla, un ejecutor de scripts y una herramienta para limpiar las bases de datos.</span><span class="sxs-lookup"><span data-stu-id="ee72d-150">It also provides a seed data script generator, a script runner, and a database cleaner.</span></span> <span data-ttu-id="ee72d-151">Para EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="ee72d-151">For EF Core: 2.</span></span>

[<span data-ttu-id="ee72d-152">Repositorio de GitHub</span><span class="sxs-lookup"><span data-stu-id="ee72d-152">GitHub repository</span></span>](https://github.com/iQuarc/Geco)

### <a name="entityframeworkcorescaffoldinghandlebars"></a><span data-ttu-id="ee72d-153">EntityFrameworkCore.Scaffolding.Handlebars</span><span class="sxs-lookup"><span data-stu-id="ee72d-153">EntityFrameworkCore.Scaffolding.Handlebars</span></span> 

<span data-ttu-id="ee72d-154">Permite la personalización de clases con ingeniería inversa a partir de una base de datos existente mediante la cadena de herramientas de Entity Framework Core con las plantillas de Handlebars.</span><span class="sxs-lookup"><span data-stu-id="ee72d-154">Allows customization of classes reverse engineered from an existing database using the Entity Framework Core toolchain with Handlebars templates.</span></span> <span data-ttu-id="ee72d-155">Para EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="ee72d-155">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="ee72d-156">Repositorio de GitHub</span><span class="sxs-lookup"><span data-stu-id="ee72d-156">GitHub repository</span></span>](https://github.com/TrackableEntities/EntityFrameworkCore.Scaffolding.Handlebars)

### <a name="neinlinqentityframeworkcore"></a><span data-ttu-id="ee72d-157">NeinLinq.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="ee72d-157">NeinLinq.EntityFrameworkCore</span></span> 

<span data-ttu-id="ee72d-158">NeinLinq amplía las características de proveedores LINQ como Entity Framework para permitir la reutilización de las funciones, la reescritura de las consultas y la creación de consultas dinámicas mediante selectores y predicados traducibles.</span><span class="sxs-lookup"><span data-stu-id="ee72d-158">NeinLinq extends LINQ providers such as Entity Framework to enable reusing functions, rewriting queries, and building dynamic queries using translatable predicates and selectors.</span></span> <span data-ttu-id="ee72d-159">Para EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="ee72d-159">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="ee72d-160">Repositorio de GitHub</span><span class="sxs-lookup"><span data-stu-id="ee72d-160">GitHub repository</span></span>](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a><span data-ttu-id="ee72d-161">Microsoft.EntityFrameworkCore.UnitOfWork</span><span class="sxs-lookup"><span data-stu-id="ee72d-161">Microsoft.EntityFrameworkCore.UnitOfWork</span></span>

<span data-ttu-id="ee72d-162">Extensión de Microsoft.EntityFrameworkCore para admitir los repositorios, los patrones de unidades de trabajo y varias bases de datos compatibles con las transacciones distribuidas.</span><span class="sxs-lookup"><span data-stu-id="ee72d-162">A plugin for Microsoft.EntityFrameworkCore to support repository, unit of work patterns, and multiple databases with distributed transaction supported.</span></span> <span data-ttu-id="ee72d-163">Para EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="ee72d-163">For EF Core: 2.</span></span>

[<span data-ttu-id="ee72d-164">Repositorio de GitHub</span><span class="sxs-lookup"><span data-stu-id="ee72d-164">GitHub repository</span></span>](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a><span data-ttu-id="ee72d-165">EFCore.BulkExtensions</span><span class="sxs-lookup"><span data-stu-id="ee72d-165">EFCore.BulkExtensions</span></span>

<span data-ttu-id="ee72d-166">Extensiones de EF Core para operaciones masivas (Insert, Update y Delete).</span><span class="sxs-lookup"><span data-stu-id="ee72d-166">EF Core extensions for Bulk operations (Insert, Update, Delete).</span></span> <span data-ttu-id="ee72d-167">Para EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="ee72d-167">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="ee72d-168">Repositorio de GitHub</span><span class="sxs-lookup"><span data-stu-id="ee72d-168">GitHub repository</span></span>](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a><span data-ttu-id="ee72d-169">Bricelam.EntityFrameworkCore.Pluralizer</span><span class="sxs-lookup"><span data-stu-id="ee72d-169">Bricelam.EntityFrameworkCore.Pluralizer</span></span>

<span data-ttu-id="ee72d-170">Agrega pluralización en tiempo de diseño.</span><span class="sxs-lookup"><span data-stu-id="ee72d-170">Adds design-time pluralization.</span></span> <span data-ttu-id="ee72d-171">Para EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="ee72d-171">For EF Core: 2.</span></span>

[<span data-ttu-id="ee72d-172">Repositorio de GitHub</span><span class="sxs-lookup"><span data-stu-id="ee72d-172">GitHub repository</span></span>](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="toolbeltentityframeworkcoreindexattribute"></a><span data-ttu-id="ee72d-173">Toolbelt.EntityFrameworkCore.IndexAttribute</span><span class="sxs-lookup"><span data-stu-id="ee72d-173">Toolbelt.EntityFrameworkCore.IndexAttribute</span></span>

<span data-ttu-id="ee72d-174">Recuperación del atributo [Index] (con extensión para la creación de modelos).</span><span class="sxs-lookup"><span data-stu-id="ee72d-174">Revival of [Index] attribute (with extension for model building).</span></span> <span data-ttu-id="ee72d-175">Para EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="ee72d-175">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="ee72d-176">Repositorio de GitHub</span><span class="sxs-lookup"><span data-stu-id="ee72d-176">GitHub repository</span></span>](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a><span data-ttu-id="ee72d-177">EfCore.InMemoryHelpers</span><span class="sxs-lookup"><span data-stu-id="ee72d-177">EfCore.InMemoryHelpers</span></span>

<span data-ttu-id="ee72d-178">Proporciona un contenedor alrededor del proveedor de la base de datos en memoria de EF Core.</span><span class="sxs-lookup"><span data-stu-id="ee72d-178">Provides a wrapper around the EF Core In-Memory Database Provider.</span></span> <span data-ttu-id="ee72d-179">Así, hace que actúe de una forma más similar a la de un proveedor relacional.</span><span class="sxs-lookup"><span data-stu-id="ee72d-179">Makes it act more like a relational provider.</span></span> <span data-ttu-id="ee72d-180">Para EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="ee72d-180">For EF Core: 2.</span></span>

[<span data-ttu-id="ee72d-181">Repositorio de GitHub</span><span class="sxs-lookup"><span data-stu-id="ee72d-181">GitHub repository</span></span>](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a><span data-ttu-id="ee72d-182">EFCore.TemporalSupport</span><span class="sxs-lookup"><span data-stu-id="ee72d-182">EFCore.TemporalSupport</span></span>

<span data-ttu-id="ee72d-183">Implementación de compatibilidad temporal.</span><span class="sxs-lookup"><span data-stu-id="ee72d-183">An implementation of temporal support.</span></span> <span data-ttu-id="ee72d-184">Para EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="ee72d-184">For EF Core: 2.</span></span>

[<span data-ttu-id="ee72d-185">Repositorio de GitHub</span><span class="sxs-lookup"><span data-stu-id="ee72d-185">GitHub repository</span></span>](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="efcoretemporaltable"></a><span data-ttu-id="ee72d-186">EfCoreTemporalTable</span><span class="sxs-lookup"><span data-stu-id="ee72d-186">EfCoreTemporalTable</span></span>

<span data-ttu-id="ee72d-187">Realice consultas temporales fácilmente en su base de datos favorita con los métodos de extensión incorporados: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`.</span><span class="sxs-lookup"><span data-stu-id="ee72d-187">Easily perform temporal queries on your favourite database using introduced extension methods: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`.</span></span> <span data-ttu-id="ee72d-188">Para EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="ee72d-188">For EF Core: 3.</span></span>

[<span data-ttu-id="ee72d-189">Repositorio de GitHub</span><span class="sxs-lookup"><span data-stu-id="ee72d-189">GitHub repository</span></span>](https://github.com/glautrou/EfCoreTemporalTable)

### <a name="efcoretimetraveler"></a><span data-ttu-id="ee72d-190">EFCore.TimeTraveler</span><span class="sxs-lookup"><span data-stu-id="ee72d-190">EFCore.TimeTraveler</span></span>

<span data-ttu-id="ee72d-191">Permita consultas completas de Entity Framework Core en el [historial temporal de SQL Server](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) mediante el código de EF Core, las entidades y las asignaciones que ya ha definido.</span><span class="sxs-lookup"><span data-stu-id="ee72d-191">Allow full-featured Entity Framework Core queries against [SQL Server Temporal History](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) using the EF Core code, entities, and mappings you already have defined.</span></span>  <span data-ttu-id="ee72d-192">Para viajar a través del tiempo, encapsule el código en `using (TemporalQuery.AsOf(targetDateTime)) {...}`.</span><span class="sxs-lookup"><span data-stu-id="ee72d-192">Travel through time by wrapping your code in `using (TemporalQuery.AsOf(targetDateTime)) {...}`.</span></span> <span data-ttu-id="ee72d-193">Para EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="ee72d-193">For EF Core: 3.</span></span>

[<span data-ttu-id="ee72d-194">Repositorio de GitHub</span><span class="sxs-lookup"><span data-stu-id="ee72d-194">GitHub repository</span></span>](https://github.com/VantageSoftware/EFCore.TimeTraveler)


### <a name="entityframeworkcoretemporaltables"></a><span data-ttu-id="ee72d-195">EntityFrameworkCore.TemporalTables</span><span class="sxs-lookup"><span data-stu-id="ee72d-195">EntityFrameworkCore.TemporalTables</span></span>

<span data-ttu-id="ee72d-196">Biblioteca de extensiones para Entity Framework Core que permite a los desarrolladores que usan SQL Server utilizar fácilmente tablas temporales.</span><span class="sxs-lookup"><span data-stu-id="ee72d-196">Extension library for Entity Framework Core which allows developers who use SQL Server to easily use temporal tables.</span></span> <span data-ttu-id="ee72d-197">Para EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="ee72d-197">For EF Core: 2.</span></span>

[<span data-ttu-id="ee72d-198">Repositorio de GitHub</span><span class="sxs-lookup"><span data-stu-id="ee72d-198">GitHub repository</span></span>](https://github.com/findulov/EntityFrameworkCore.TemporalTables)


### <a name="entityframeworkcorecacheable"></a><span data-ttu-id="ee72d-199">EntityFrameworkCore.Cacheable</span><span class="sxs-lookup"><span data-stu-id="ee72d-199">EntityFrameworkCore.Cacheable</span></span>

<span data-ttu-id="ee72d-200">Caché de consulta de segundo nivel y alto rendimiento.</span><span class="sxs-lookup"><span data-stu-id="ee72d-200">A high-performance second-level query cache.</span></span> <span data-ttu-id="ee72d-201">Para EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="ee72d-201">For EF Core: 2.</span></span>

[<span data-ttu-id="ee72d-202">Repositorio de GitHub</span><span class="sxs-lookup"><span data-stu-id="ee72d-202">GitHub repository</span></span>](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entity-framework-plus"></a><span data-ttu-id="ee72d-203">Entity Framework Plus</span><span class="sxs-lookup"><span data-stu-id="ee72d-203">Entity Framework Plus</span></span>

<span data-ttu-id="ee72d-204">Amplía su DbContext con características como: Incluir filtro, Auditoría, Cache, Consulta de futuro, Batch Delete, Actualización por lotes y más.</span><span class="sxs-lookup"><span data-stu-id="ee72d-204">Extends your DbContext with features such as: Include Filter, Auditing, Caching, Query Future, Batch Delete, Batch Update, and more.</span></span> <span data-ttu-id="ee72d-205">Para EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="ee72d-205">For EF Core: 2, 3.</span></span>

<span data-ttu-id="ee72d-206">[Sitio web](https://entityframework-plus.net/)
[Repositorio de GitHub](https://github.com/zzzprojects/EntityFramework-Plus)</span><span class="sxs-lookup"><span data-stu-id="ee72d-206">[Website](https://entityframework-plus.net/)
[GitHub repository](https://github.com/zzzprojects/EntityFramework-Plus)</span></span>

### <a name="entity-framework-extensions"></a><span data-ttu-id="ee72d-207">Extensiones de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="ee72d-207">Entity Framework Extensions</span></span>

<span data-ttu-id="ee72d-208">Extiende su DbContext con operaciones masivas de alto rendimiento: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge y más.</span><span class="sxs-lookup"><span data-stu-id="ee72d-208">Extends your DbContext with high-performance bulk operations: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge, and more.</span></span> <span data-ttu-id="ee72d-209">Para EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="ee72d-209">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="ee72d-210">Sitio web</span><span class="sxs-lookup"><span data-stu-id="ee72d-210">Website</span></span>](https://entityframework-extensions.net/)
