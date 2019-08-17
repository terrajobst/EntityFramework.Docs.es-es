---
title: Migración de EF6 a requisitos de validación EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 07caa39e8a56db71e493331ea9f0286309abc52a
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565349"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a><span data-ttu-id="4a6da-102">Antes de migrar de EF6 a EF Core: Validar los requisitos de la aplicación</span><span class="sxs-lookup"><span data-stu-id="4a6da-102">Before porting from EF6 to EF Core: Validate your Application's Requirements</span></span>

<span data-ttu-id="4a6da-103">Antes de comenzar el proceso de portabilidad, es importante validar que EF Core cumple los requisitos de acceso a los datos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4a6da-103">Before you start the porting process it is important to validate that EF Core meets the data access requirements for your application.</span></span>

## <a name="missing-features"></a><span data-ttu-id="4a6da-104">Características que faltan</span><span class="sxs-lookup"><span data-stu-id="4a6da-104">Missing features</span></span>

<span data-ttu-id="4a6da-105">Asegúrese de que EF Core tenga todas las características que necesita para usar en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4a6da-105">Make sure that EF Core has all the features you need to use in your application.</span></span> <span data-ttu-id="4a6da-106">Vea [comparación de características](../features.md) para obtener una comparación detallada de cómo el conjunto de características en EF Core se compara con EF6.</span><span class="sxs-lookup"><span data-stu-id="4a6da-106">See [Feature Comparison](../features.md) for a detailed comparison of how the feature set in EF Core compares to EF6.</span></span> <span data-ttu-id="4a6da-107">Si faltan las características necesarias, asegúrese de que puede compensar la falta de estas características antes de migrar a EF Core.</span><span class="sxs-lookup"><span data-stu-id="4a6da-107">If any required features are missing, ensure that you can compensate for the lack of these features before porting to EF Core.</span></span>

## <a name="behavior-changes"></a><span data-ttu-id="4a6da-108">Cambios de comportamiento</span><span class="sxs-lookup"><span data-stu-id="4a6da-108">Behavior changes</span></span>

<span data-ttu-id="4a6da-109">Se trata de una lista no exhaustiva de algunos cambios de comportamiento entre EF6 y EF Core.</span><span class="sxs-lookup"><span data-stu-id="4a6da-109">This is a non-exhaustive list of some changes in behavior between EF6 and EF Core.</span></span> <span data-ttu-id="4a6da-110">Es importante tener esto en cuenta como el puerto de la aplicación, ya que pueden cambiar la forma en que se comporta la aplicación, pero no se mostrarán como errores de compilación después de cambiar a EF Core.</span><span class="sxs-lookup"><span data-stu-id="4a6da-110">It is important to keep these in mind as your port your application as they may change the way your application behaves, but will not show up as compilation errors after swapping to EF Core.</span></span>

### <a name="dbsetaddattach-and-graph-behavior"></a><span data-ttu-id="4a6da-111">DbSet. Add/Attach y comportamiento del grafo</span><span class="sxs-lookup"><span data-stu-id="4a6da-111">DbSet.Add/Attach and graph behavior</span></span>

<span data-ttu-id="4a6da-112">En EF6, llamar `DbSet.Add()` a en una entidad da como resultado una búsqueda recursiva de todas las entidades a las que se hace referencia en sus propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="4a6da-112">In EF6, calling `DbSet.Add()` on an entity results in a recursive search for all entities referenced in its navigation properties.</span></span> <span data-ttu-id="4a6da-113">Las entidades que se encuentren y a las que no se haya realizado el seguimiento en el contexto, también se marcarán como agregadas.</span><span class="sxs-lookup"><span data-stu-id="4a6da-113">Any entities that are found, and are not already tracked by the context, are also marked as added.</span></span> <span data-ttu-id="4a6da-114">`DbSet.Attach()`se comporta de la misma forma, salvo que todas las entidades se marcan como sin cambios.</span><span class="sxs-lookup"><span data-stu-id="4a6da-114">`DbSet.Attach()` behaves the same, except all entities are marked as unchanged.</span></span>

<span data-ttu-id="4a6da-115">**EF Core realiza una búsqueda recursiva similar, pero con algunas reglas ligeramente diferentes.**</span><span class="sxs-lookup"><span data-stu-id="4a6da-115">**EF Core performs a similar recursive search, but with some slightly different rules.**</span></span>

*  <span data-ttu-id="4a6da-116">La entidad raíz siempre está en el estado solicitado (agregado para `DbSet.Add` y sin cambios para `DbSet.Attach`).</span><span class="sxs-lookup"><span data-stu-id="4a6da-116">The root entity is always in the requested state (added for `DbSet.Add` and unchanged for `DbSet.Attach`).</span></span>

*  <span data-ttu-id="4a6da-117">**Para las entidades que se encuentran durante la búsqueda recursiva de propiedades de navegación:**</span><span class="sxs-lookup"><span data-stu-id="4a6da-117">**For entities that are found during the recursive search of navigation properties:**</span></span>

    *  <span data-ttu-id="4a6da-118">**Si la clave principal de la entidad se ha generado en el almacén**</span><span class="sxs-lookup"><span data-stu-id="4a6da-118">**If the primary key of the entity is store generated**</span></span>

        * <span data-ttu-id="4a6da-119">Si la clave principal no se establece en un valor, el estado se establece en Added.</span><span class="sxs-lookup"><span data-stu-id="4a6da-119">If the primary key is not set to a value, the state is set to added.</span></span> <span data-ttu-id="4a6da-120">El valor de clave principal se considera "no establecido" si se le asigna el valor predeterminado de CLR para el tipo de propiedad ( `0` por `int`ejemplo, `string`para, `null` para, etc.).</span><span class="sxs-lookup"><span data-stu-id="4a6da-120">The primary key value is considered "not set" if it is assigned the CLR default value for the property type (for example, `0` for `int`, `null` for `string`, etc.).</span></span>

        * <span data-ttu-id="4a6da-121">Si la clave principal se establece en un valor, el estado se establece en Unchanged.</span><span class="sxs-lookup"><span data-stu-id="4a6da-121">If the primary key is set to a value, the state is set to unchanged.</span></span>

    *  <span data-ttu-id="4a6da-122">Si la clave principal no se genera en la base de datos, la entidad se coloca en el mismo estado que la raíz.</span><span class="sxs-lookup"><span data-stu-id="4a6da-122">If the primary key is not database generated, the entity is put in the same state as the root.</span></span>

### <a name="code-first-database-initialization"></a><span data-ttu-id="4a6da-123">Inicialización de Code First base de datos</span><span class="sxs-lookup"><span data-stu-id="4a6da-123">Code First database initialization</span></span>

<span data-ttu-id="4a6da-124">**EF6 tiene una cantidad significativa de magia que se realiza en torno a la selección de la conexión de base de datos y la inicialización de la base de datos. Algunas de estas reglas incluyen:**</span><span class="sxs-lookup"><span data-stu-id="4a6da-124">**EF6 has a significant amount of magic it performs around selecting the database connection and initializing the database. Some of these rules include:**</span></span>

* <span data-ttu-id="4a6da-125">Si no se realiza ninguna configuración, EF6 seleccionará una base de datos en SQL Express o LocalDb.</span><span class="sxs-lookup"><span data-stu-id="4a6da-125">If no configuration is performed, EF6 will select a database on SQL Express or LocalDb.</span></span>

* <span data-ttu-id="4a6da-126">Si una cadena de conexión con el mismo nombre que el contexto está en el `App/Web.config` archivo de aplicaciones, se usará esta conexión.</span><span class="sxs-lookup"><span data-stu-id="4a6da-126">If a connection string with the same name as the context is in the applications `App/Web.config` file, this connection will be used.</span></span>

* <span data-ttu-id="4a6da-127">Si la base de datos no existe, se crea.</span><span class="sxs-lookup"><span data-stu-id="4a6da-127">If the database does not exist, it is created.</span></span>

* <span data-ttu-id="4a6da-128">Si no existe ninguna tabla del modelo en la base de datos, se agrega el esquema del modelo actual a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4a6da-128">If none of the tables from the model exist in the database, the schema for the current model is added to the database.</span></span> <span data-ttu-id="4a6da-129">Si se habilitan las migraciones, se usan para crear la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4a6da-129">If migrations are enabled, then they are used to create the database.</span></span>

* <span data-ttu-id="4a6da-130">Si la base de datos existe y EF6 creó previamente el esquema, se comprueba la compatibilidad del esquema con el modelo actual.</span><span class="sxs-lookup"><span data-stu-id="4a6da-130">If the database exists and EF6 had previously created the schema, then the schema is checked for compatibility with the current model.</span></span> <span data-ttu-id="4a6da-131">Se produce una excepción si el modelo ha cambiado desde que se creó el esquema.</span><span class="sxs-lookup"><span data-stu-id="4a6da-131">An exception is thrown if the model has changed since the schema was created.</span></span>

<span data-ttu-id="4a6da-132">**EF Core no realiza ninguna de estas instrucciones mágicas.**</span><span class="sxs-lookup"><span data-stu-id="4a6da-132">**EF Core does not perform any of this magic.**</span></span>

* <span data-ttu-id="4a6da-133">La conexión de base de datos debe configurarse explícitamente en el código.</span><span class="sxs-lookup"><span data-stu-id="4a6da-133">The database connection must be explicitly configured in code.</span></span>

* <span data-ttu-id="4a6da-134">No se realiza ninguna inicialización.</span><span class="sxs-lookup"><span data-stu-id="4a6da-134">No initialization is performed.</span></span> <span data-ttu-id="4a6da-135">Debe usar `DbContext.Database.Migrate()` para aplicar las Migraciones (o `DbContext.Database.EnsureCreated()` y `EnsureDeleted()` para crear o eliminar la base de datos sin usar migraciones).</span><span class="sxs-lookup"><span data-stu-id="4a6da-135">You must use `DbContext.Database.Migrate()` to apply migrations (or `DbContext.Database.EnsureCreated()` and `EnsureDeleted()` to create/delete the database without using migrations).</span></span>

### <a name="code-first-table-naming-convention"></a><span data-ttu-id="4a6da-136">Code First Convención de nomenclatura de tabla</span><span class="sxs-lookup"><span data-stu-id="4a6da-136">Code First table naming convention</span></span>

<span data-ttu-id="4a6da-137">EF6 ejecuta el nombre de la clase de entidad a través de un servicio de pluralización para calcular el nombre de tabla predeterminado al que está asignada la entidad.</span><span class="sxs-lookup"><span data-stu-id="4a6da-137">EF6 runs the entity class name through a pluralization service to calculate the default table name that the entity is mapped to.</span></span>

<span data-ttu-id="4a6da-138">EF Core usa el nombre de la `DbSet` propiedad en la que se expone la entidad en el contexto derivado.</span><span class="sxs-lookup"><span data-stu-id="4a6da-138">EF Core uses the name of the `DbSet` property that the entity is exposed in on the derived context.</span></span> <span data-ttu-id="4a6da-139">Si la entidad no tiene una `DbSet` propiedad, se utiliza el nombre de clase.</span><span class="sxs-lookup"><span data-stu-id="4a6da-139">If the entity does not have a `DbSet` property, then the class name is used.</span></span>
