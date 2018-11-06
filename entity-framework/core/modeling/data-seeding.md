---
title: Propagación de datos - EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/02/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 791f7afff36aac52fe2ffdc16ab580db22011b99
ms.sourcegitcommit: 082946dcaa1ee5174e692dbfe53adeed40609c6a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2018
ms.locfileid: "51028101"
---
# <a name="data-seeding"></a><span data-ttu-id="f7023-102">Propagación de datos</span><span class="sxs-lookup"><span data-stu-id="f7023-102">Data Seeding</span></span>

<span data-ttu-id="f7023-103">Propagación de datos es el proceso de rellenar una base de datos con un conjunto inicial de datos.</span><span class="sxs-lookup"><span data-stu-id="f7023-103">Data seeding is the process of populating a database with an initial set of data.</span></span>

<span data-ttu-id="f7023-104">Hay varias maneras de que esto puede realizarse en EF Core:</span><span class="sxs-lookup"><span data-stu-id="f7023-104">There are several ways this can be accomplished in EF Core:</span></span>
* <span data-ttu-id="f7023-105">Datos de inicialización del modelo</span><span class="sxs-lookup"><span data-stu-id="f7023-105">Model seed data</span></span>
* <span data-ttu-id="f7023-106">Personalización de la migración manual</span><span class="sxs-lookup"><span data-stu-id="f7023-106">Manual migration customization</span></span>
* <span data-ttu-id="f7023-107">Lógica de inicialización personalizado</span><span class="sxs-lookup"><span data-stu-id="f7023-107">Custom initialization logic</span></span>

## <a name="model-seed-data"></a><span data-ttu-id="f7023-108">Datos de inicialización del modelo</span><span class="sxs-lookup"><span data-stu-id="f7023-108">Model seed data</span></span>

> [!NOTE]
> <span data-ttu-id="f7023-109">Esta característica es nueva en EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="f7023-109">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="f7023-110">A diferencia de en EF6 en EF Core, la propagación de datos puede asociarse con un tipo de entidad como parte de la configuración del modelo.</span><span class="sxs-lookup"><span data-stu-id="f7023-110">Unlike in EF6, in EF Core, seeding data can be associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="f7023-111">A continuación, EF Core [migraciones](xref:core/managing-schemas/migrations/index) puede calcular automáticamente qué insertar, actualizar o eliminar la necesidad de las operaciones que se aplicará al actualizar la base de datos a una nueva versión del modelo.</span><span class="sxs-lookup"><span data-stu-id="f7023-111">Then EF Core [migrations](xref:core/managing-schemas/migrations/index) can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

> [!NOTE]
> <span data-ttu-id="f7023-112">Las migraciones solo considera que los cambios en el modelo al determinar qué operación debe realizarse para obtener los datos de inicialización en el estado deseado.</span><span class="sxs-lookup"><span data-stu-id="f7023-112">Migrations only considers model changes when determining what operation should be performed to get the seed data into the desired state.</span></span> <span data-ttu-id="f7023-113">Por lo tanto es posible que se perderán los cambios en los datos que se realiza fuera de las migraciones o se producirá un error.</span><span class="sxs-lookup"><span data-stu-id="f7023-113">Thus any changes to the data performed outside of migrations might be lost or cause an error.</span></span>

<span data-ttu-id="f7023-114">Por ejemplo, se configurarán los datos de inicialización para un `Blog` en `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="f7023-114">As an example, this will configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[BlogSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="f7023-115">Para agregar las entidades que tienen una relación de los valores de clave externos debe especificarse:</span><span class="sxs-lookup"><span data-stu-id="f7023-115">To add entities that have a relationship the foreign key values need to be specified:</span></span>

[!code-csharp[PostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

<span data-ttu-id="f7023-116">Si el tipo de entidad tiene propiedades en una clase anónima puede utilizarse para proporcionar los valores de estado de sombra:</span><span class="sxs-lookup"><span data-stu-id="f7023-116">If the entity type has any properties in shadow state an anonymous class can be used to provide the values:</span></span>

[!code-csharp[AnonymousPostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=AnonymousPostSeed)]

<span data-ttu-id="f7023-117">Propiedad de entidad tipos pueden propagarse de forma similar:</span><span class="sxs-lookup"><span data-stu-id="f7023-117">Owned entity types can be seeded in a similar fashion:</span></span>

[!code-csharp[OwnedTypeSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=OwnedTypeSeed)]

<span data-ttu-id="f7023-118">Consulte la [proyecto de ejemplo completa](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) para obtener más contexto.</span><span class="sxs-lookup"><span data-stu-id="f7023-118">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) for more context.</span></span>

<span data-ttu-id="f7023-119">Una vez que los datos se ha agregado al modelo, [migraciones](xref:core/managing-schemas/migrations/index) debe usarse para aplicar los cambios.</span><span class="sxs-lookup"><span data-stu-id="f7023-119">Once the data has been added to the model, [migrations](xref:core/managing-schemas/migrations/index) should be used to apply the changes.</span></span>

> [!TIP]
> <span data-ttu-id="f7023-120">Si tiene que aplicar las migraciones como parte de una implementación automatizada, puede [crear un script SQL](xref:core/managing-schemas/migrations/index#generate-sql-scripts) que puede obtener una vista previa antes de la ejecución.</span><span class="sxs-lookup"><span data-stu-id="f7023-120">If you need to apply migrations as part of an automated deployment you can [create a SQL script](xref:core/managing-schemas/migrations/index#generate-sql-scripts) that can be previewed before execution.</span></span>

<span data-ttu-id="f7023-121">Como alternativa, puede usar `context.Database.EnsureCreated()` para crear una nueva base de datos que contiene los datos de inicialización, por ejemplo, para una base de datos de prueba o cuando se usa el proveedor en memoria o cualquier base de datos sin relación.</span><span class="sxs-lookup"><span data-stu-id="f7023-121">Alternatively, you can use `context.Database.EnsureCreated()` to create a new database containing the seed data, for example for a test database or when using the in-memory provider or any non-relation database.</span></span> <span data-ttu-id="f7023-122">Observe que si ya existe la base de datos, `EnsureCreated()` ni actualizará el esquema ni los datos de inicialización en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="f7023-122">Note that if the database already exists, `EnsureCreated()` will neither update the schema nor the seed data in the database.</span></span> <span data-ttu-id="f7023-123">Bases de datos relacionales no debería llamar a `EnsureCreated()` si tiene previsto usar migraciones.</span><span class="sxs-lookup"><span data-stu-id="f7023-123">For relational databases you shouldn't call `EnsureCreated()` if you plan to use Migrations.</span></span>

<span data-ttu-id="f7023-124">Las migraciones se administra este tipo de datos de inicialización y la secuencia de comandos para actualizar los datos que ya está en la base de datos se debe generar sin necesidad de conectarse a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="f7023-124">This type of seed data is managed by migrations and the script to update the data that's already in the database needs to be generated without connecting to the database.</span></span> <span data-ttu-id="f7023-125">Esto impone algunas restricciones:</span><span class="sxs-lookup"><span data-stu-id="f7023-125">This imposes some restrictions:</span></span>
* <span data-ttu-id="f7023-126">El valor de clave principal debe especificarse incluso si se genera normalmente por la base de datos.</span><span class="sxs-lookup"><span data-stu-id="f7023-126">The primary key value needs to be specified even if it's usually generated by the database.</span></span> <span data-ttu-id="f7023-127">Se utilizará para detectar cambios en los datos entre las migraciones.</span><span class="sxs-lookup"><span data-stu-id="f7023-127">It will be used to detect data changes between migrations.</span></span>
* <span data-ttu-id="f7023-128">Previamente los datos inicializados se quitará si se cambia la clave principal de ninguna manera.</span><span class="sxs-lookup"><span data-stu-id="f7023-128">Previously seeded data will be removed if the primary key is changed in any way.</span></span>

<span data-ttu-id="f7023-129">Por lo tanto, esta característica resulta especialmente útil para datos estáticos que no se espera que cambie fuera de las migraciones y no dependen de cualquier otra cosa en la base de datos, por ejemplo los códigos postales.</span><span class="sxs-lookup"><span data-stu-id="f7023-129">Therefore this feature is most useful for static data that's not expected to change outside of migrations and does not depend on anything else in the database, for example ZIP codes.</span></span>

<span data-ttu-id="f7023-130">Si su escenario incluye alguno de los siguientes, se recomienda usar lógica de inicialización personalizadas descrita en la última sección:</span><span class="sxs-lookup"><span data-stu-id="f7023-130">If your scenario includes any of the following it is recommended to use custom initialization logic described in the last section:</span></span>
* <span data-ttu-id="f7023-131">Datos temporales para las pruebas</span><span class="sxs-lookup"><span data-stu-id="f7023-131">Temporary data for testing</span></span>
* <span data-ttu-id="f7023-132">Datos que dependen de estado de la base de datos</span><span class="sxs-lookup"><span data-stu-id="f7023-132">Data that depends on database state</span></span>
* <span data-ttu-id="f7023-133">Datos que tengan valores de clave que va a generar la base de datos, incluidas las entidades que utilizan las claves alternativas como la identidad</span><span class="sxs-lookup"><span data-stu-id="f7023-133">Data that needs key values to be generated by the database, including entities that use alternate keys as the identity</span></span>
* <span data-ttu-id="f7023-134">Datos que requiere la transformación personalizada (que no está controlado por [conversiones de valor](xref:core/modeling/value-conversions)), por ejemplo, algunos algoritmos de hash de contraseña</span><span class="sxs-lookup"><span data-stu-id="f7023-134">Data that requires custom transformation (that is not handled by [value conversions](xref:core/modeling/value-conversions)), such as some password hashing</span></span>
* <span data-ttu-id="f7023-135">Datos que requieren las llamadas a API externa, como la creación de usuarios y roles de ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="f7023-135">Data that requires calls to external API, such as ASP.NET Core Identity roles and users creation</span></span>

## <a name="manual-migration-customization"></a><span data-ttu-id="f7023-136">Personalización de la migración manual</span><span class="sxs-lookup"><span data-stu-id="f7023-136">Manual migration customization</span></span>

<span data-ttu-id="f7023-137">Cuando se agrega una migración los cambios a los datos especificados con `HasData` se transforman en llamadas a `InsertData()`, `UpdateData()`, y `DeleteData()`.</span><span class="sxs-lookup"><span data-stu-id="f7023-137">When a migration is added the changes to the data specified with `HasData` are transformed to calls to `InsertData()`, `UpdateData()`, and `DeleteData()`.</span></span> <span data-ttu-id="f7023-138">Una manera de solucionar algunas de las limitaciones de `HasData` es agregar manualmente estas llamadas o [operaciones personalizadas](xref:core/managing-schemas/migrations/operations) para la migración en su lugar.</span><span class="sxs-lookup"><span data-stu-id="f7023-138">One way of working around some of the limitations of `HasData` is to manually add these calls or [custom operations](xref:core/managing-schemas/migrations/operations) to the migration instead.</span></span>

[!code-csharp[CustomInsert](../../../samples/core/Modeling/DataSeeding/Migrations/20181102235626_Initial.cs?name=CustomInsert)]

## <a name="custom-initialization-logic"></a><span data-ttu-id="f7023-139">Lógica de inicialización personalizado</span><span class="sxs-lookup"><span data-stu-id="f7023-139">Custom initialization logic</span></span>

<span data-ttu-id="f7023-140">Una manera sencilla y eficaz para realizar la propagación de datos es usar [ `DbContext.SaveChanges()` ](xref:core/saving/index) antes de la aplicación principal lógica comienza la ejecución.</span><span class="sxs-lookup"><span data-stu-id="f7023-140">A straightforward and powerful way to perform data seeding is to use [`DbContext.SaveChanges()`](xref:core/saving/index) before the main application logic begins execution.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataSeeding/Program.cs?name=CustomSeeding)]

> [!WARNING]
> <span data-ttu-id="f7023-141">El código de propagación no debe formar parte de la ejecución de aplicación normal ya que esto puede causar problemas de simultaneidad cuando varias instancias se ejecutan y también podría requerir que las aplicaciones deben tener permiso para modificar el esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="f7023-141">The seeding code should not be part of the normal app execution as this can cause concurrency issues when multiple instances are running and would also require the app having permission to modify the database schema.</span></span>

<span data-ttu-id="f7023-142">Dependiendo de las restricciones de la implementación se puede ejecutar el código de inicialización de maneras diferentes:</span><span class="sxs-lookup"><span data-stu-id="f7023-142">Depending on the constraints of your deployment the initialization code can be executed in different ways:</span></span>
* <span data-ttu-id="f7023-143">Ejecutar la aplicación de inicialización localmente</span><span class="sxs-lookup"><span data-stu-id="f7023-143">Running the initialization app locally</span></span>
* <span data-ttu-id="f7023-144">Implementación de la aplicación de inicialización con la aplicación principal, invocar la rutina de inicialización y deshabilitación o eliminación de la aplicación de la inicialización.</span><span class="sxs-lookup"><span data-stu-id="f7023-144">Deploying the initialization app with the main app, invoking the initialization routine and disabling or removing the initialization app.</span></span>

<span data-ttu-id="f7023-145">Normalmente, esto puede automatizarse mediante [perfiles de publicación](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/visual-studio-publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="f7023-145">This can usually be automated by using [publish profiles](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/visual-studio-publish-profiles).</span></span>
