---
title: Herramientas y extensiones - EF Core
author: ErikEJ
ms.date: 01/07/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: 0c9671eb77181d85cd493341cd1abf842d13fb0e
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181269"
---
# <a name="ef-core-tools--extensions"></a><span data-ttu-id="ebeb1-102">Herramientas y extensiones de EF Core</span><span class="sxs-lookup"><span data-stu-id="ebeb1-102">EF Core Tools & Extensions</span></span>

<span data-ttu-id="ebeb1-103">Estas herramientas y extensiones proporcionan más funcionalidades para Entity Framework Core 2.0 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="ebeb1-103">These tools and extensions provide additional functionality for Entity Framework Core 2.0 and later.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="ebeb1-104">Las extensiones están compiladas a partir de una gran variedad de orígenes, por lo que su mantenimiento no está incluido en el proyecto Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="ebeb1-104">Extensions are built by a variety of sources and aren't maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="ebeb1-105">En lo que respecta a las extensiones de terceros y para asegurarse de que cumplan sus requisitos, no se olvide de evaluar la calidad, las licencias, la compatibilidad, el soporte técnico, etc.</span><span class="sxs-lookup"><span data-stu-id="ebeb1-105">When considering a third party extension, be sure to evaluate its quality, licensing, compatibility, support, etc. to ensure it meets your requirements.</span></span>

## <a name="tools"></a><span data-ttu-id="ebeb1-106">Herramientas</span><span class="sxs-lookup"><span data-stu-id="ebeb1-106">Tools</span></span>

### <a name="llblgen-pro"></a><span data-ttu-id="ebeb1-107">LLBLGen Pro</span><span class="sxs-lookup"><span data-stu-id="ebeb1-107">LLBLGen Pro</span></span>

<span data-ttu-id="ebeb1-108">LLBLGen Pro es una solución de modelado de entidad compatible con Entity Framework y Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="ebeb1-108">LLBLGen Pro is an entity modeling solution with support for Entity Framework and Entity Framework Core.</span></span> <span data-ttu-id="ebeb1-109">Permite definir fácilmente el modelo de entidad y asignarlo a la base de datos mediante Database First o Model First, de modo que pueda empezar a escribir consultas de inmediato.</span><span class="sxs-lookup"><span data-stu-id="ebeb1-109">It lets you easily define your entity model and map it to your database, using database first or model first, so you can get started writing queries right away.</span></span>

[<span data-ttu-id="ebeb1-110">Sitio web</span><span class="sxs-lookup"><span data-stu-id="ebeb1-110">Website</span></span>](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a><span data-ttu-id="ebeb1-111">Devart Entity Developer</span><span class="sxs-lookup"><span data-stu-id="ebeb1-111">Devart Entity Developer</span></span>

<span data-ttu-id="ebeb1-112">Entity Developer es un potente diseñador ORM para ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access y LINQ to SQL.</span><span class="sxs-lookup"><span data-stu-id="ebeb1-112">Entity Developer is a powerful ORM designer for ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access, and LINQ to SQL.</span></span> <span data-ttu-id="ebeb1-113">Permite diseñar modelos EF Core de forma visual mediante los enfoques Database First y Model First, así como generar código C# y de Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="ebeb1-113">It supports designing EF Core models visually, using model first or database first approaches, and C# or Visual Basic code generation.</span></span> 

[<span data-ttu-id="ebeb1-114">Sitio web</span><span class="sxs-lookup"><span data-stu-id="ebeb1-114">Website</span></span>](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a><span data-ttu-id="ebeb1-115">EF Core Power Tools</span><span class="sxs-lookup"><span data-stu-id="ebeb1-115">EF Core Power Tools</span></span>

<span data-ttu-id="ebeb1-116">EF Core Power Tools es una extensión de Visual Studio 2017 que expone varias tareas de tiempo de diseño de EF Core en una interfaz de usuario sencilla.</span><span class="sxs-lookup"><span data-stu-id="ebeb1-116">EF Core Power Tools is a Visual Studio 2017 extension that exposes various EF Core design-time tasks in a simple user interface.</span></span> <span data-ttu-id="ebeb1-117">Incluye técnicas de ingeniería inversa de DbContext y clases de entidades a partir de bases de datos existentes y [DACPAC de SQL Server](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), así como la administración de la migración de bases de datos y la visualización de modelos.</span><span class="sxs-lookup"><span data-stu-id="ebeb1-117">It includes reverse engineering of DbContext and entity classes from existing databases and [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), management of database migrations, and model visualizations.</span></span>

[<span data-ttu-id="ebeb1-118">Wiki de GitHub</span><span class="sxs-lookup"><span data-stu-id="ebeb1-118">GitHub wiki</span></span>](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a><span data-ttu-id="ebeb1-119">Editor visual de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="ebeb1-119">Entity Framework Visual Editor</span></span>

<span data-ttu-id="ebeb1-120">El Editor de objetos visuales de Entity Framework es una extensión de Visual Studio que agrega un diseñador ORM que permite generar objetos visuales de las clases EF 6 y EF Core.</span><span class="sxs-lookup"><span data-stu-id="ebeb1-120">Entity Framework Visual Editor is a Visual Studio extension that adds an ORM designer for visual design of EF 6, and EF Core classes.</span></span> <span data-ttu-id="ebeb1-121">El código se genera mediante plantillas T4, por lo que se puede personalizar.</span><span class="sxs-lookup"><span data-stu-id="ebeb1-121">Code is generated using T4 templates so can be customized to suit any needs.</span></span> <span data-ttu-id="ebeb1-122">Asimismo, admite enumeraciones y asociaciones de herencia, unidireccionales y bidireccionales, y permite asignar colores a las clases y agregar bloques de texto para explicar ciertas partes del diseño que puedan resultar difíciles de comprender.</span><span class="sxs-lookup"><span data-stu-id="ebeb1-122">It supports inheritance, unidirectional and bidirectional associations, enumerations, and the ability to color-code your classes and add text blocks to explain potentially arcane parts of your design.</span></span>

[<span data-ttu-id="ebeb1-123">Marketplace</span><span class="sxs-lookup"><span data-stu-id="ebeb1-123">Marketplace</span></span>](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a><span data-ttu-id="ebeb1-124">CatFactory</span><span class="sxs-lookup"><span data-stu-id="ebeb1-124">CatFactory</span></span>

<span data-ttu-id="ebeb1-125">CatFactory es un motor de scaffolding para .NET Core que permite automatizar la generación de clases DbContext, entidades, opciones de configuración de la asignación y clases de repositorios de una base de datos SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ebeb1-125">CatFactory is a scaffolding engine for .NET Core that can automate the generation of DbContext classes, entities, mapping configurations, and repository classes from a SQL Server database.</span></span>

[<span data-ttu-id="ebeb1-126">Repositorio de GitHub</span><span class="sxs-lookup"><span data-stu-id="ebeb1-126">GitHub repository</span></span>](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a><span data-ttu-id="ebeb1-127">LoreSoft's Entity Framework Core Generator</span><span class="sxs-lookup"><span data-stu-id="ebeb1-127">LoreSoft's Entity Framework Core Generator</span></span>

<span data-ttu-id="ebeb1-128">Entity Framework Core Generator (efg) es una herramienta de la CLI de .NET Core que permite generar modelos EF Core a partir de una base de datos existente. Es similar a `dotnet ef dbcontext scaffold`, pero también admite la [regeneración](https://efg.loresoft.com/en/latest/regeneration/) de código seguro mediante el reemplazo de la región o el análisis de los archivos de asignación.</span><span class="sxs-lookup"><span data-stu-id="ebeb1-128">Entity Framework Core Generator (efg) is a .NET Core CLI tool that can generate EF Core models from an existing database, much like `dotnet ef dbcontext scaffold`, but it also supports safe code [regeneration](https://efg.loresoft.com/en/latest/regeneration/) via region replacement or by parsing mapping files.</span></span> <span data-ttu-id="ebeb1-129">Asimismo, la herramienta permite generar modelos de vista, efectuar la validación y crear el código de los asignadores de objetos.</span><span class="sxs-lookup"><span data-stu-id="ebeb1-129">This tool supports generating view models, validation, and object mapper code.</span></span> 

<span data-ttu-id="ebeb1-130">[Tutorial](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentación](https://efg.loresoft.com/en/latest/)</span><span class="sxs-lookup"><span data-stu-id="ebeb1-130">[Tutorial](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentation](https://efg.loresoft.com/en/latest/)</span></span>

## <a name="extensions"></a><span data-ttu-id="ebeb1-131">Extensiones</span><span class="sxs-lookup"><span data-stu-id="ebeb1-131">Extensions</span></span>

### <a name="microsoftentityframeworkcoreautohistory"></a><span data-ttu-id="ebeb1-132">Microsoft.EntityFrameworkCore.AutoHistory</span><span class="sxs-lookup"><span data-stu-id="ebeb1-132">Microsoft.EntityFrameworkCore.AutoHistory</span></span>

<span data-ttu-id="ebeb1-133">Biblioteca de extensiones que permite registrar automáticamente los cambios en los datos realizados por EF Core e incluirlos en una tabla a modo de historial.</span><span class="sxs-lookup"><span data-stu-id="ebeb1-133">A plugin library that enables automatically recording the data changes performed by EF Core into a history table.</span></span>

[<span data-ttu-id="ebeb1-134">Repositorio de GitHub</span><span class="sxs-lookup"><span data-stu-id="ebeb1-134">GitHub repository</span></span>](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a><span data-ttu-id="ebeb1-135">Microsoft.EntityFrameworkCore.DynamicLinq</span><span class="sxs-lookup"><span data-stu-id="ebeb1-135">Microsoft.EntityFrameworkCore.DynamicLinq</span></span>

<span data-ttu-id="ebeb1-136">Puerto de .NET Core y .NET Standard de System.Linq.Dynamic que incluye compatibilidad asincrónica con EF Core.</span><span class="sxs-lookup"><span data-stu-id="ebeb1-136">A .NET Core / .NET Standard port of System.Linq.Dynamic that includes async support with EF Core.</span></span>
<span data-ttu-id="ebeb1-137">Elemento System.Linq.Dynamic (muestra de Microsoft) en el que se ilustra cómo construir consultas LINQ de forma dinámica a partir de expresiones de cadena, en lugar de utilizar el código.</span><span class="sxs-lookup"><span data-stu-id="ebeb1-137">System.Linq.Dynamic originated as a Microsoft sample that shows how to construct LINQ queries dynamically from string expressions rather than code.</span></span>

[<span data-ttu-id="ebeb1-138">Repositorio de GitHub</span><span class="sxs-lookup"><span data-stu-id="ebeb1-138">GitHub repository</span></span>](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efsecondlevelcachecore"></a><span data-ttu-id="ebeb1-139">EFSecondLevelCache.Core</span><span class="sxs-lookup"><span data-stu-id="ebeb1-139">EFSecondLevelCache.Core</span></span>

<span data-ttu-id="ebeb1-140">Extensión que permite almacenar los resultados de las consultas EF Core en una caché de segundo nivel. De este modo, al volver a ejecutar las mismas cadenas, se pueden recuperar los datos directamente de la caché sin acceder a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="ebeb1-140">An extension that enables storing the results of EF Core queries into a second-level cache, so that subsequent executions of the same queries can avoid accessing the database and retrieve the data directly from the cache.</span></span>

[<span data-ttu-id="ebeb1-141">Repositorio de GitHub</span><span class="sxs-lookup"><span data-stu-id="ebeb1-141">GitHub repository</span></span>](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="entityframeworkcoreprimarykey"></a><span data-ttu-id="ebeb1-142">EntityFrameworkCore.PrimaryKey</span><span class="sxs-lookup"><span data-stu-id="ebeb1-142">EntityFrameworkCore.PrimaryKey</span></span>

<span data-ttu-id="ebeb1-143">Esta biblioteca permite recuperar los valores de una clave principal (incluidas las claves compuestas) de una entidad como un diccionario.</span><span class="sxs-lookup"><span data-stu-id="ebeb1-143">This library allows retrieving the values of primary key (including composite keys) from any entity as a dictionary.</span></span>

[<span data-ttu-id="ebeb1-144">Repositorio de GitHub</span><span class="sxs-lookup"><span data-stu-id="ebeb1-144">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcoretypedoriginalvalues"></a><span data-ttu-id="ebeb1-145">EntityFrameworkCore.TypedOriginalValues</span><span class="sxs-lookup"><span data-stu-id="ebeb1-145">EntityFrameworkCore.TypedOriginalValues</span></span>

<span data-ttu-id="ebeb1-146">Esta biblioteca habilita el acceso fuertemente tipado a los valores originales de las propiedades de las entidades.</span><span class="sxs-lookup"><span data-stu-id="ebeb1-146">This library enables strongly typed access to the original values of entity properties.</span></span> 

[<span data-ttu-id="ebeb1-147">Repositorio de GitHub</span><span class="sxs-lookup"><span data-stu-id="ebeb1-147">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a><span data-ttu-id="ebeb1-148">Geco</span><span class="sxs-lookup"><span data-stu-id="ebeb1-148">Geco</span></span>

<span data-ttu-id="ebeb1-149">Geco (Generator Console) es un generador de código muy sencillo que se basa en un proyecto de consola. Para generar el código, se ejecuta en .NET Core y utiliza cadenas C# interpoladas.</span><span class="sxs-lookup"><span data-stu-id="ebeb1-149">Geco (Generator Console) is a simple code generator based on a console project, that runs on .NET Core and uses C# interpolated strings for code generation.</span></span> <span data-ttu-id="ebeb1-150">Geco incluye un generador de modelos de ingeniería inversa para EF Core que admite la pluralización, la singularización y las plantillas editables.</span><span class="sxs-lookup"><span data-stu-id="ebeb1-150">Geco includes a reverse model generator for EF Core with support for pluralization, singularization, and editable templates.</span></span> <span data-ttu-id="ebeb1-151">También proporciona un generador de scripts de datos semilla, un ejecutor de scripts y una herramienta para limpiar las bases de datos.</span><span class="sxs-lookup"><span data-stu-id="ebeb1-151">It also provides a seed data script generator, a script runner, and a database cleaner.</span></span>

[<span data-ttu-id="ebeb1-152">Repositorio de GitHub</span><span class="sxs-lookup"><span data-stu-id="ebeb1-152">GitHub repository</span></span>](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a><span data-ttu-id="ebeb1-153">LinqKit.Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="ebeb1-153">LinqKit.Microsoft.EntityFrameworkCore</span></span>

<span data-ttu-id="ebeb1-154">LinqKit.Microsoft.EntityFrameworkCore es una versión de la biblioteca LINQKit compatible con EF Core.</span><span class="sxs-lookup"><span data-stu-id="ebeb1-154">LinqKit.Microsoft.EntityFrameworkCore is an EF Core-compatible version of the LINQKit library.</span></span> <span data-ttu-id="ebeb1-155">LINQKit es un conjunto de extensiones gratuito para los usuarios avanzados de LINQ to SQL y Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ebeb1-155">LINQKit is a free set of extensions for LINQ to SQL and Entity Framework power users.</span></span> <span data-ttu-id="ebeb1-156">Proporciona funcionalidades avanzadas como la creación dinámica de expresiones de predicado y el uso de variables de expresiones en las subconsultas.</span><span class="sxs-lookup"><span data-stu-id="ebeb1-156">It enables advanced functionality like dynamic building of predicate expressions, and using expression variables in subqueries.</span></span>  

[<span data-ttu-id="ebeb1-157">Repositorio de GitHub</span><span class="sxs-lookup"><span data-stu-id="ebeb1-157">GitHub repository</span></span>](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a><span data-ttu-id="ebeb1-158">NeinLinq.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="ebeb1-158">NeinLinq.EntityFrameworkCore</span></span>

<span data-ttu-id="ebeb1-159">NeinLinq amplía las características de proveedores LINQ como Entity Framework para permitir la reutilización de las funciones, la reescritura de las consultas y la creación de consultas dinámicas mediante selectores y predicados traducibles.</span><span class="sxs-lookup"><span data-stu-id="ebeb1-159">NeinLinq extends LINQ providers such as Entity Framework to enable reusing functions, rewriting queries, and building dynamic queries using translatable predicates and selectors.</span></span>

[<span data-ttu-id="ebeb1-160">Repositorio de GitHub</span><span class="sxs-lookup"><span data-stu-id="ebeb1-160">GitHub repository</span></span>](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a><span data-ttu-id="ebeb1-161">Microsoft.EntityFrameworkCore.UnitOfWork</span><span class="sxs-lookup"><span data-stu-id="ebeb1-161">Microsoft.EntityFrameworkCore.UnitOfWork</span></span>

<span data-ttu-id="ebeb1-162">Extensión de Microsoft.EntityFrameworkCore para admitir los repositorios, los patrones de unidades de trabajo y varias bases de datos compatibles con las transacciones distribuidas.</span><span class="sxs-lookup"><span data-stu-id="ebeb1-162">A plugin for Microsoft.EntityFrameworkCore to support repository, unit of work patterns, and multiple databases with distributed transaction supported.</span></span>

[<span data-ttu-id="ebeb1-163">Repositorio de GitHub</span><span class="sxs-lookup"><span data-stu-id="ebeb1-163">GitHub repository</span></span>](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a><span data-ttu-id="ebeb1-164">EFCore.BulkExtensions</span><span class="sxs-lookup"><span data-stu-id="ebeb1-164">EFCore.BulkExtensions</span></span>

<span data-ttu-id="ebeb1-165">Extensiones de EF Core para operaciones masivas (Insert, Update y Delete).</span><span class="sxs-lookup"><span data-stu-id="ebeb1-165">EF Core extensions for Bulk operations (Insert, Update, Delete).</span></span>

[<span data-ttu-id="ebeb1-166">Repositorio de GitHub</span><span class="sxs-lookup"><span data-stu-id="ebeb1-166">GitHub repository</span></span>](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a><span data-ttu-id="ebeb1-167">Bricelam.EntityFrameworkCore.Pluralizer</span><span class="sxs-lookup"><span data-stu-id="ebeb1-167">Bricelam.EntityFrameworkCore.Pluralizer</span></span>

<span data-ttu-id="ebeb1-168">Agrega pluralización de tiempo de diseño a EF Core.</span><span class="sxs-lookup"><span data-stu-id="ebeb1-168">Adds design-time pluralization to EF Core.</span></span>

[<span data-ttu-id="ebeb1-169">Repositorio de GitHub</span><span class="sxs-lookup"><span data-stu-id="ebeb1-169">GitHub repository</span></span>](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="pomelofoundationpomeloentityframeworkcoreextensionstosql"></a><span data-ttu-id="ebeb1-170">PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql</span><span class="sxs-lookup"><span data-stu-id="ebeb1-170">PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql</span></span>

<span data-ttu-id="ebeb1-171">Método de extensión sencillo que obtiene la instrucción SQL que EF Core generaría para una consulta LINQ determinada en casos simples.</span><span class="sxs-lookup"><span data-stu-id="ebeb1-171">A simple extension method that obtains the SQL statement EF Core would generate for a given LINQ query in simple scenarios.</span></span> <span data-ttu-id="ebeb1-172">El uso del método ToSql está limitado a casos simples, dado que EF Core puede generar más de una instrucción SQL para una única consulta LINQ, así como distintas instrucciones SQL en función de los valores de los parámetros.</span><span class="sxs-lookup"><span data-stu-id="ebeb1-172">The ToSql method is limited to simple scenarios because EF Core can generate more than one SQL statement for a single LINQ query, and different SQL statements depending on parameter values.</span></span>

[<span data-ttu-id="ebeb1-173">Repositorio de GitHub</span><span class="sxs-lookup"><span data-stu-id="ebeb1-173">GitHub repository</span></span>](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql)

### <a name="toolbeltentityframeworkcoreindexattribute"></a><span data-ttu-id="ebeb1-174">Toolbelt.EntityFrameworkCore.IndexAttribute</span><span class="sxs-lookup"><span data-stu-id="ebeb1-174">Toolbelt.EntityFrameworkCore.IndexAttribute</span></span>

<span data-ttu-id="ebeb1-175">Recuperación del atributo [Index] para EF Core (con extensión para la creación de modelos).</span><span class="sxs-lookup"><span data-stu-id="ebeb1-175">Revival of [Index] attribute for EF Core (with extension for model building).</span></span>

[<span data-ttu-id="ebeb1-176">Repositorio de GitHub</span><span class="sxs-lookup"><span data-stu-id="ebeb1-176">GitHub repository</span></span>](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a><span data-ttu-id="ebeb1-177">EfCore.InMemoryHelpers</span><span class="sxs-lookup"><span data-stu-id="ebeb1-177">EfCore.InMemoryHelpers</span></span>

<span data-ttu-id="ebeb1-178">Proporciona un contenedor alrededor del proveedor de la base de datos en memoria de EF Core.</span><span class="sxs-lookup"><span data-stu-id="ebeb1-178">Provides a wrapper around the EF Core In-Memory Database Provider.</span></span> <span data-ttu-id="ebeb1-179">Así, hace que actúe de una forma más similar a la de un proveedor relacional.</span><span class="sxs-lookup"><span data-stu-id="ebeb1-179">Makes it act more like a relational provider.</span></span>

[<span data-ttu-id="ebeb1-180">Repositorio de GitHub</span><span class="sxs-lookup"><span data-stu-id="ebeb1-180">GitHub repository</span></span>](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a><span data-ttu-id="ebeb1-181">EFCore.TemporalSupport</span><span class="sxs-lookup"><span data-stu-id="ebeb1-181">EFCore.TemporalSupport</span></span>

<span data-ttu-id="ebeb1-182">Implementación de compatibilidad temporal para EF Core.</span><span class="sxs-lookup"><span data-stu-id="ebeb1-182">An implementation of temporal support for EF Core.</span></span>

[<span data-ttu-id="ebeb1-183">Repositorio de GitHub</span><span class="sxs-lookup"><span data-stu-id="ebeb1-183">GitHub repository</span></span>](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="entityframeworkcorecacheable"></a><span data-ttu-id="ebeb1-184">EntityFrameworkCore.Cacheable</span><span class="sxs-lookup"><span data-stu-id="ebeb1-184">EntityFrameworkCore.Cacheable</span></span>

<span data-ttu-id="ebeb1-185">Caché de consulta de segundo nivel y alto rendimiento para EF Core.</span><span class="sxs-lookup"><span data-stu-id="ebeb1-185">A high-performance second-level query cache for EF Core.</span></span>

[<span data-ttu-id="ebeb1-186">Repositorio de GitHub</span><span class="sxs-lookup"><span data-stu-id="ebeb1-186">GitHub repository</span></span>](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entity-framework-plus"></a><span data-ttu-id="ebeb1-187">Entity Framework Plus</span><span class="sxs-lookup"><span data-stu-id="ebeb1-187">Entity Framework Plus</span></span>

<span data-ttu-id="ebeb1-188">Amplía su DbContext con características como: Incluir filtro, Auditoría, Cache, Consulta de futuro, Batch Delete, Actualización por lotes y más.</span><span class="sxs-lookup"><span data-stu-id="ebeb1-188">Extends your DbContext with features such as: Include Filter, Auditing, Caching, Query Future, Batch Delete, Batch Update, and more.</span></span>

<span data-ttu-id="ebeb1-189">[Sitio web](https://entityframework-plus.net/)
[Repositorio de GitHub](https://github.com/zzzprojects/EntityFramework-Plus)</span><span class="sxs-lookup"><span data-stu-id="ebeb1-189">[Website](https://entityframework-plus.net/)
[GitHub repository](https://github.com/zzzprojects/EntityFramework-Plus)</span></span>

### <a name="entity-framework-extensions"></a><span data-ttu-id="ebeb1-190">Extensiones de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="ebeb1-190">Entity Framework Extensions</span></span>

<span data-ttu-id="ebeb1-191">Extiende su DbContext con operaciones masivas de alto rendimiento: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge y más.</span><span class="sxs-lookup"><span data-stu-id="ebeb1-191">Extends your DbContext with high-performance bulk operations: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge, and more.</span></span>

[<span data-ttu-id="ebeb1-192">Sitio web</span><span class="sxs-lookup"><span data-stu-id="ebeb1-192">Website</span></span>](https://entityframework-extensions.net/)

### <a name="reconciler"></a><span data-ttu-id="ebeb1-193">Reconciliador</span><span class="sxs-lookup"><span data-stu-id="ebeb1-193">Reconciler</span></span>

<span data-ttu-id="ebeb1-194">Actualice un gráfico de entidades en el almacén a uno específico mediante la inserción, actualización y eliminación de las entidades correspondientes.</span><span class="sxs-lookup"><span data-stu-id="ebeb1-194">Update an entity graph in store to a given one by inserting, updating and removing the respective entities.</span></span>

[<span data-ttu-id="ebeb1-195">Repositorio de GitHub</span><span class="sxs-lookup"><span data-stu-id="ebeb1-195">GitHub repository</span></span>](https://github.com/jtheisen/reconciler)
