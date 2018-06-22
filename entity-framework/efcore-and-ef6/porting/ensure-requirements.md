---
title: 'Migrar desde EF6 EF básica: validar los requisitos'
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 2f45039e63546d266ec6ce0bfa66ef7e9fb3d7e7
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052865"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a><span data-ttu-id="8a739-102">Antes de migrar desde EF6 Core EF: validar los requisitos de su aplicación</span><span class="sxs-lookup"><span data-stu-id="8a739-102">Before porting from EF6 to EF Core: Validate your Application's Requirements</span></span>

<span data-ttu-id="8a739-103">Antes de empezar el proceso de migración es importante validar que EF Core cumple los requisitos de acceso de datos para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8a739-103">Before you start the porting process it is important to validate that EF Core meets the data access requirements for your application.</span></span>

## <a name="missing-features"></a><span data-ttu-id="8a739-104">Características que faltan</span><span class="sxs-lookup"><span data-stu-id="8a739-104">Missing features</span></span>

<span data-ttu-id="8a739-105">Asegúrese de que EF Core incluye todas las características que se debe usar en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8a739-105">Make sure that EF Core has all the features you need to use in your application.</span></span> <span data-ttu-id="8a739-106">Vea [comparación de características](../features.md) para obtener una comparación detallada de cómo se comparan el conjunto de características de núcleo de EF con EF6.</span><span class="sxs-lookup"><span data-stu-id="8a739-106">See [Feature Comparison](../features.md) for a detailed comparison of how the feature set in EF Core compares to EF6.</span></span> <span data-ttu-id="8a739-107">Si faltan las características necesarias, asegúrese de que es posible compensar la falta de estas características antes de migrar a EF Core.</span><span class="sxs-lookup"><span data-stu-id="8a739-107">If any required features are missing, ensure that you can compensate for the lack of these features before porting to EF Core.</span></span>

## <a name="behavior-changes"></a><span data-ttu-id="8a739-108">Cambios de comportamiento</span><span class="sxs-lookup"><span data-stu-id="8a739-108">Behavior changes</span></span>

<span data-ttu-id="8a739-109">Se trata de una lista no exhaustiva de algunos cambios de comportamiento entre EF6 y EF Core.</span><span class="sxs-lookup"><span data-stu-id="8a739-109">This is a non-exhaustive list of some changes in behavior between EF6 and EF Core.</span></span> <span data-ttu-id="8a739-110">Es importante tener en cuenta como el puerto de la aplicación tal y como puede cambiar la manera en que se comporta de la aplicación, pero no aparecerán como errores de compilación después de intercambiar a EF Core.</span><span class="sxs-lookup"><span data-stu-id="8a739-110">It is important to keep these in mind as your port your application as they may change the way your application behaves, but will not show up as compilation errors after swapping to EF Core.</span></span>

### <a name="dbsetaddattach-and-graph-behavior"></a><span data-ttu-id="8a739-111">Comportamiento de DbSet.Add/Attach y gráfico</span><span class="sxs-lookup"><span data-stu-id="8a739-111">DbSet.Add/Attach and graph behavior</span></span>

<span data-ttu-id="8a739-112">En EF6, una llamada a `DbSet.Add()` en el resultado de una entidad en una búsqueda recursiva para todas las entidades que se hace referencia en sus propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="8a739-112">In EF6, calling `DbSet.Add()` on an entity results in a recursive search for all entities referenced in its navigation properties.</span></span> <span data-ttu-id="8a739-113">Todas las entidades que se encuentran y no tienen seguimiento por el contexto, son también se marca como agregado.</span><span class="sxs-lookup"><span data-stu-id="8a739-113">Any entities that are found, and are not already tracked by the context, are also be marked as added.</span></span> <span data-ttu-id="8a739-114">`DbSet.Attach()`se comporta igual, salvo que todas las entidades se marcan como sin cambios.</span><span class="sxs-lookup"><span data-stu-id="8a739-114">`DbSet.Attach()` behaves the same, except all entities are marked as unchanged.</span></span>

<span data-ttu-id="8a739-115">**Núcleo EF realiza una búsqueda recursiva similar, pero con algunas reglas ligeramente diferentes.**</span><span class="sxs-lookup"><span data-stu-id="8a739-115">**EF Core performs a similar recursive search, but with some slightly different rules.**</span></span>

*  <span data-ttu-id="8a739-116">La entidad raíz siempre está en el estado solicitado (agregado para `DbSet.Add` y ha modificado para `DbSet.Attach`).</span><span class="sxs-lookup"><span data-stu-id="8a739-116">The root entity is always in the requested state (added for `DbSet.Add` and unchanged for `DbSet.Attach`).</span></span>

*  <span data-ttu-id="8a739-117">**Para las entidades que se encuentran durante la búsqueda recursiva de propiedades de navegación:**</span><span class="sxs-lookup"><span data-stu-id="8a739-117">**For entities that are found during the recursive search of navigation properties:**</span></span>

    *  <span data-ttu-id="8a739-118">**Si la clave principal de la entidad es almacén generado**</span><span class="sxs-lookup"><span data-stu-id="8a739-118">**If the primary key of the entity is store generated**</span></span>

        * <span data-ttu-id="8a739-119">Si la clave principal no se establece en un valor, el estado se establece a agregado.</span><span class="sxs-lookup"><span data-stu-id="8a739-119">If the primary key is not set to a value, the state is set to added.</span></span> <span data-ttu-id="8a739-120">El valor de clave principal se considera "no configurada" si se asigna el valor predeterminado CLR para el tipo de propiedad (es decir, `0` para `int`, `null` para `string`, etcetera.).</span><span class="sxs-lookup"><span data-stu-id="8a739-120">The primary key value is considered "not set" if it is assigned the CLR default value for the property type (i.e. `0` for `int`, `null` for `string`, etc.).</span></span>

        * <span data-ttu-id="8a739-121">Si la clave principal se establece en un valor, el estado se establece en sin cambios.</span><span class="sxs-lookup"><span data-stu-id="8a739-121">If the primary key is set to a value, the state is set to unchanged.</span></span>

    *  <span data-ttu-id="8a739-122">Si la clave principal no está generado de la base de datos, la entidad se coloca en el mismo estado que la raíz.</span><span class="sxs-lookup"><span data-stu-id="8a739-122">If the primary key is not database generated, the entity is put in the same state as the root.</span></span>

### <a name="code-first-database-initialization"></a><span data-ttu-id="8a739-123">Código de la primera inicialización de la base de datos</span><span class="sxs-lookup"><span data-stu-id="8a739-123">Code First database initialization</span></span>

<span data-ttu-id="8a739-124">**EF6 tiene una cantidad significativa de magia realiza alrededor de la selección de la conexión de base de datos e inicializar la base de datos. Algunas de estas reglas incluyen:**</span><span class="sxs-lookup"><span data-stu-id="8a739-124">**EF6 has a significant amount of magic it performs around selecting the database connection and initializing the database. Some of these rules include:**</span></span>

* <span data-ttu-id="8a739-125">Si no se realiza ninguna configuración, EF6 seleccionará una base de datos SQL Express o LocalDb.</span><span class="sxs-lookup"><span data-stu-id="8a739-125">If no configuration is performed, EF6 will select a database on SQL Express or LocalDb.</span></span>

* <span data-ttu-id="8a739-126">Si es una cadena de conexión con el mismo nombre que el contexto de las aplicaciones `App/Web.config` archivo, se usará esta conexión.</span><span class="sxs-lookup"><span data-stu-id="8a739-126">If a connection string with the same name as the context is in the applications `App/Web.config` file, this connection will be used.</span></span>

* <span data-ttu-id="8a739-127">Si la base de datos no existe, se crea.</span><span class="sxs-lookup"><span data-stu-id="8a739-127">If the database does not exist, it is created.</span></span>

* <span data-ttu-id="8a739-128">Si no existe ninguna de las tablas del modelo en la base de datos, el esquema para el modelo actual se agrega a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="8a739-128">If none of the tables from the model exist in the database, the schema for the current model is added to the database.</span></span> <span data-ttu-id="8a739-129">Si las migraciones están habilitadas, se utilizan para crear la base de datos.</span><span class="sxs-lookup"><span data-stu-id="8a739-129">If migrations are enabled, then they are used to create the database.</span></span>

* <span data-ttu-id="8a739-130">Si existe la base de datos y EF6 había creado anteriormente el esquema, se comprueba el esquema para la compatibilidad con el modelo actual.</span><span class="sxs-lookup"><span data-stu-id="8a739-130">If the database exists and EF6 had previously created the schema, then the schema is checked for compatibility with the current model.</span></span> <span data-ttu-id="8a739-131">Se produce una excepción si el modelo ha cambiado desde que se creó el esquema.</span><span class="sxs-lookup"><span data-stu-id="8a739-131">An exception is thrown if the model has changed since the schema was created.</span></span>

<span data-ttu-id="8a739-132">**Núcleo EF no realiza ninguna este mágico.**</span><span class="sxs-lookup"><span data-stu-id="8a739-132">**EF Core does not perform any of this magic.**</span></span>

* <span data-ttu-id="8a739-133">La conexión de base de datos debe configurarse explícitamente en el código.</span><span class="sxs-lookup"><span data-stu-id="8a739-133">The database connection must be explicitly configured in code.</span></span>

* <span data-ttu-id="8a739-134">No se realiza ninguna inicialización.</span><span class="sxs-lookup"><span data-stu-id="8a739-134">No initialization is performed.</span></span> <span data-ttu-id="8a739-135">Debe usar `DbContext.Database.Migrate()` para aplicar las migraciones (o `DbContext.Database.EnsureCreated()` y `EnsureDeleted()` para crear o eliminar la base de datos sin usar migraciones).</span><span class="sxs-lookup"><span data-stu-id="8a739-135">You must use `DbContext.Database.Migrate()` to apply migrations (or `DbContext.Database.EnsureCreated()` and `EnsureDeleted()` to create/delete the database without using migrations).</span></span>

### <a name="code-first-table-naming-convention"></a><span data-ttu-id="8a739-136">Convención de nomenclatura de primera tabla de código</span><span class="sxs-lookup"><span data-stu-id="8a739-136">Code First table naming convention</span></span>

<span data-ttu-id="8a739-137">EF6 ejecuta el nombre de clase de entidad a través de un servicio de pluralización para calcular el nombre de tabla predeterminado que está asignada la entidad a.</span><span class="sxs-lookup"><span data-stu-id="8a739-137">EF6 runs the entity class name through a pluralization service to calculate the default table name that the entity is mapped to.</span></span>

<span data-ttu-id="8a739-138">EF Core utiliza el nombre de la `DbSet` propiedad que la entidad está expuesta en el contexto derivado.</span><span class="sxs-lookup"><span data-stu-id="8a739-138">EF Core uses the name of the `DbSet` property that the entity is exposed in on the derived context.</span></span> <span data-ttu-id="8a739-139">Si la entidad no tiene un `DbSet` se utiliza la propiedad y, a continuación, el nombre de clase.</span><span class="sxs-lookup"><span data-stu-id="8a739-139">If the entity does not have a `DbSet` property, then the class name is used.</span></span>
