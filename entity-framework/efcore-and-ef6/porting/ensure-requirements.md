---
title: 'Portabilidad de EF6 a EF Core: validar los requisitos'
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 65bdc8bb9574d37db697aa47c8e8c480cefcb4f7
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949119"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a><span data-ttu-id="e9cfb-102">Antes de portabilidad de EF6 a EF Core: validar los requisitos de la aplicación</span><span class="sxs-lookup"><span data-stu-id="e9cfb-102">Before porting from EF6 to EF Core: Validate your Application's Requirements</span></span>

<span data-ttu-id="e9cfb-103">Antes de iniciar el proceso de conversión es importante validar que EF Core cumple los requisitos de acceso de datos para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e9cfb-103">Before you start the porting process it is important to validate that EF Core meets the data access requirements for your application.</span></span>

## <a name="missing-features"></a><span data-ttu-id="e9cfb-104">Características que faltan</span><span class="sxs-lookup"><span data-stu-id="e9cfb-104">Missing features</span></span>

<span data-ttu-id="e9cfb-105">Asegúrese de que EF Core tiene todas las características que necesita para usar en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e9cfb-105">Make sure that EF Core has all the features you need to use in your application.</span></span> <span data-ttu-id="e9cfb-106">Consulte [comparación de características](../features.md) para ver una comparación detallada de cómo se comparan el conjunto de características de EF Core a EF6.</span><span class="sxs-lookup"><span data-stu-id="e9cfb-106">See [Feature Comparison](../features.md) for a detailed comparison of how the feature set in EF Core compares to EF6.</span></span> <span data-ttu-id="e9cfb-107">Si faltan las características necesarias, asegúrese de que puede compensar la falta de estas características antes de migrar a EF Core.</span><span class="sxs-lookup"><span data-stu-id="e9cfb-107">If any required features are missing, ensure that you can compensate for the lack of these features before porting to EF Core.</span></span>

## <a name="behavior-changes"></a><span data-ttu-id="e9cfb-108">Cambios de comportamiento</span><span class="sxs-lookup"><span data-stu-id="e9cfb-108">Behavior changes</span></span>

<span data-ttu-id="e9cfb-109">Se trata de una lista no exhaustiva de algunos cambios de comportamiento entre EF6 y EF Core.</span><span class="sxs-lookup"><span data-stu-id="e9cfb-109">This is a non-exhaustive list of some changes in behavior between EF6 and EF Core.</span></span> <span data-ttu-id="e9cfb-110">Es importante recordar esto en mente como el puerto de la aplicación tal como puede cambiar la forma en que se comporta la aplicación, pero no se mostrará como errores de compilación después de intercambiar a EF Core.</span><span class="sxs-lookup"><span data-stu-id="e9cfb-110">It is important to keep these in mind as your port your application as they may change the way your application behaves, but will not show up as compilation errors after swapping to EF Core.</span></span>

### <a name="dbsetaddattach-and-graph-behavior"></a><span data-ttu-id="e9cfb-111">Comportamiento DbSet.Add/Attach y graph</span><span class="sxs-lookup"><span data-stu-id="e9cfb-111">DbSet.Add/Attach and graph behavior</span></span>

<span data-ttu-id="e9cfb-112">En EF6, una llamada a `DbSet.Add()` en los resultados de una entidad en una búsqueda recursiva para todas las entidades que se hace referencia en sus propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="e9cfb-112">In EF6, calling `DbSet.Add()` on an entity results in a recursive search for all entities referenced in its navigation properties.</span></span> <span data-ttu-id="e9cfb-113">Todas las entidades que se encuentran y ya no contarán según el contexto, son también se marca como agregado.</span><span class="sxs-lookup"><span data-stu-id="e9cfb-113">Any entities that are found, and are not already tracked by the context, are also be marked as added.</span></span> <span data-ttu-id="e9cfb-114">`DbSet.Attach()` se comporta igual, salvo que todas las entidades se marcan como sin cambios.</span><span class="sxs-lookup"><span data-stu-id="e9cfb-114">`DbSet.Attach()` behaves the same, except all entities are marked as unchanged.</span></span>

<span data-ttu-id="e9cfb-115">**EF Core realiza una búsqueda recursiva similar, pero con algunas reglas ligeramente distintas.**</span><span class="sxs-lookup"><span data-stu-id="e9cfb-115">**EF Core performs a similar recursive search, but with some slightly different rules.**</span></span>

*  <span data-ttu-id="e9cfb-116">La entidad raíz siempre está en el estado solicitado (agregado para `DbSet.Add` y sin modificaciones para `DbSet.Attach`).</span><span class="sxs-lookup"><span data-stu-id="e9cfb-116">The root entity is always in the requested state (added for `DbSet.Add` and unchanged for `DbSet.Attach`).</span></span>

*  <span data-ttu-id="e9cfb-117">**Para las entidades que se encuentran durante la búsqueda recursiva de las propiedades de navegación:**</span><span class="sxs-lookup"><span data-stu-id="e9cfb-117">**For entities that are found during the recursive search of navigation properties:**</span></span>

    *  <span data-ttu-id="e9cfb-118">**Si la clave principal de la entidad es generado de almacén**</span><span class="sxs-lookup"><span data-stu-id="e9cfb-118">**If the primary key of the entity is store generated**</span></span>

        * <span data-ttu-id="e9cfb-119">Si la clave principal no se establece en un valor, el estado se establece a agregado.</span><span class="sxs-lookup"><span data-stu-id="e9cfb-119">If the primary key is not set to a value, the state is set to added.</span></span> <span data-ttu-id="e9cfb-120">El valor de clave principal se considera "sin establecer" si se asigna el valor predeterminado CLR para el tipo de propiedad (por ejemplo, `0` para `int`, `null` para `string`, etcetera.).</span><span class="sxs-lookup"><span data-stu-id="e9cfb-120">The primary key value is considered "not set" if it is assigned the CLR default value for the property type (for example, `0` for `int`, `null` for `string`, etc.).</span></span>

        * <span data-ttu-id="e9cfb-121">Si la clave principal se establece en un valor, el estado se establece en sin cambios.</span><span class="sxs-lookup"><span data-stu-id="e9cfb-121">If the primary key is set to a value, the state is set to unchanged.</span></span>

    *  <span data-ttu-id="e9cfb-122">Si la clave principal no es una base de datos generada, la entidad se coloca en el mismo estado que la raíz.</span><span class="sxs-lookup"><span data-stu-id="e9cfb-122">If the primary key is not database generated, the entity is put in the same state as the root.</span></span>

### <a name="code-first-database-initialization"></a><span data-ttu-id="e9cfb-123">Código de inicialización primera base de datos</span><span class="sxs-lookup"><span data-stu-id="e9cfb-123">Code First database initialization</span></span>

<span data-ttu-id="e9cfb-124">**EF6 tiene una cantidad significativa de magia que realiza en torno a la selección de la conexión de base de datos e inicializar la base de datos. Algunas de estas reglas incluyen:**</span><span class="sxs-lookup"><span data-stu-id="e9cfb-124">**EF6 has a significant amount of magic it performs around selecting the database connection and initializing the database. Some of these rules include:**</span></span>

* <span data-ttu-id="e9cfb-125">Si no se realiza ninguna configuración, EF6 seleccionará una base de datos SQL Express o LocalDb.</span><span class="sxs-lookup"><span data-stu-id="e9cfb-125">If no configuration is performed, EF6 will select a database on SQL Express or LocalDb.</span></span>

* <span data-ttu-id="e9cfb-126">Si es una cadena de conexión con el mismo nombre que el contexto en las aplicaciones `App/Web.config` archivo, se usará esta conexión.</span><span class="sxs-lookup"><span data-stu-id="e9cfb-126">If a connection string with the same name as the context is in the applications `App/Web.config` file, this connection will be used.</span></span>

* <span data-ttu-id="e9cfb-127">Si la base de datos no existe, se crea.</span><span class="sxs-lookup"><span data-stu-id="e9cfb-127">If the database does not exist, it is created.</span></span>

* <span data-ttu-id="e9cfb-128">Si no existe ninguna de las tablas del modelo en la base de datos, se agrega el esquema para el modelo actual a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="e9cfb-128">If none of the tables from the model exist in the database, the schema for the current model is added to the database.</span></span> <span data-ttu-id="e9cfb-129">Si están habilitadas las migraciones, se utilizan para crear la base de datos.</span><span class="sxs-lookup"><span data-stu-id="e9cfb-129">If migrations are enabled, then they are used to create the database.</span></span>

* <span data-ttu-id="e9cfb-130">Si existe la base de datos y EF6 había creado anteriormente en el esquema, se comprueba el esquema para la compatibilidad con el modelo actual.</span><span class="sxs-lookup"><span data-stu-id="e9cfb-130">If the database exists and EF6 had previously created the schema, then the schema is checked for compatibility with the current model.</span></span> <span data-ttu-id="e9cfb-131">Se produce una excepción si el modelo ha cambiado desde que se creó el esquema.</span><span class="sxs-lookup"><span data-stu-id="e9cfb-131">An exception is thrown if the model has changed since the schema was created.</span></span>

<span data-ttu-id="e9cfb-132">**EF Core no lleva a cabo cualquiera de esta instrucción mágica.**</span><span class="sxs-lookup"><span data-stu-id="e9cfb-132">**EF Core does not perform any of this magic.**</span></span>

* <span data-ttu-id="e9cfb-133">La conexión de base de datos debe configurarse explícitamente en el código.</span><span class="sxs-lookup"><span data-stu-id="e9cfb-133">The database connection must be explicitly configured in code.</span></span>

* <span data-ttu-id="e9cfb-134">No se realiza ninguna inicialización.</span><span class="sxs-lookup"><span data-stu-id="e9cfb-134">No initialization is performed.</span></span> <span data-ttu-id="e9cfb-135">Debe usar `DbContext.Database.Migrate()` para aplicar las migraciones (o `DbContext.Database.EnsureCreated()` y `EnsureDeleted()` para crear o eliminar la base de datos sin usar migraciones).</span><span class="sxs-lookup"><span data-stu-id="e9cfb-135">You must use `DbContext.Database.Migrate()` to apply migrations (or `DbContext.Database.EnsureCreated()` and `EnsureDeleted()` to create/delete the database without using migrations).</span></span>

### <a name="code-first-table-naming-convention"></a><span data-ttu-id="e9cfb-136">Convención de nomenclatura de primera tabla de código</span><span class="sxs-lookup"><span data-stu-id="e9cfb-136">Code First table naming convention</span></span>

<span data-ttu-id="e9cfb-137">EF6 se ejecuta el nombre de clase de entidad a través de un servicio de pluralización para calcular el nombre de tabla predeterminado asignado a la entidad.</span><span class="sxs-lookup"><span data-stu-id="e9cfb-137">EF6 runs the entity class name through a pluralization service to calculate the default table name that the entity is mapped to.</span></span>

<span data-ttu-id="e9cfb-138">EF Core usa el nombre de la `DbSet` propiedad que la entidad está expuesta en el contexto derivado.</span><span class="sxs-lookup"><span data-stu-id="e9cfb-138">EF Core uses the name of the `DbSet` property that the entity is exposed in on the derived context.</span></span> <span data-ttu-id="e9cfb-139">Si la entidad no tiene un `DbSet` se utiliza la propiedad y, a continuación, el nombre de clase.</span><span class="sxs-lookup"><span data-stu-id="e9cfb-139">If the entity does not have a `DbSet` property, then the class name is used.</span></span>
