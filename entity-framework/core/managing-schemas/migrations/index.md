---
title: Migraciones - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: 4a5d6f3798c7af7597f95cebea1aeb9e5e58d277
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996527"
---
<a name="migrations"></a><span data-ttu-id="27989-102">Migraciones</span><span class="sxs-lookup"><span data-stu-id="27989-102">Migrations</span></span>
==========
<span data-ttu-id="27989-103">Las migraciones proporcionan una manera de aplicar cambios de esquema de forma incremental a la base de datos para mantenerla sincronizada con el modelo de EF Core al tiempo que se conservan los datos existentes en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="27989-103">Migrations provide a way to incrementally apply schema changes to the database to keep it in sync with your EF Core model while preserving existing data in the database.</span></span>

<a name="creating-the-database"></a><span data-ttu-id="27989-104">Creación de la base de datos</span><span class="sxs-lookup"><span data-stu-id="27989-104">Creating the database</span></span>
---------------------
<span data-ttu-id="27989-105">Una vez [definido el modelo inicial][1], es el momento de crear la base de datos.</span><span class="sxs-lookup"><span data-stu-id="27989-105">After you've [defined your initial model][1], it's time to create the database.</span></span> <span data-ttu-id="27989-106">Para ello, agregue una migración inicial.</span><span class="sxs-lookup"><span data-stu-id="27989-106">To do this, add an initial migration.</span></span>
<span data-ttu-id="27989-107">Instale [las herramientas de EF Core][2] y ejecute el comando adecuado.</span><span class="sxs-lookup"><span data-stu-id="27989-107">Install the [EF Core Tools][2] and run the appropriate command.</span></span>

``` powershell
Add-Migration InitialCreate
```
``` Console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="27989-108">Se agregan tres archivos al proyecto en el directorio **Migraciones**:</span><span class="sxs-lookup"><span data-stu-id="27989-108">Three files are added to your project under the **Migrations** directory:</span></span>

* <span data-ttu-id="27989-109">**00000000000000_InitialCreate.cs**: archivo principal de migraciones.</span><span class="sxs-lookup"><span data-stu-id="27989-109">**00000000000000_InitialCreate.cs**--The main migrations file.</span></span> <span data-ttu-id="27989-110">Contiene las operaciones necesarias para aplicar la migración (en `Up()`) y para revertirla (en `Down()`).</span><span class="sxs-lookup"><span data-stu-id="27989-110">Contains the operations necessary to apply the migration (in `Up()`) and to revert it (in `Down()`).</span></span>
* <span data-ttu-id="27989-111">**00000000000000_InitialCreate.Designer.cs**: archivo de metadatos de migraciones.</span><span class="sxs-lookup"><span data-stu-id="27989-111">**00000000000000_InitialCreate.Designer.cs**--The migrations metadata file.</span></span> <span data-ttu-id="27989-112">Contiene información que usa EF.</span><span class="sxs-lookup"><span data-stu-id="27989-112">Contains information used by EF.</span></span>
* <span data-ttu-id="27989-113">**MyContextModelSnapshot.cs**: instantánea del modelo actual.</span><span class="sxs-lookup"><span data-stu-id="27989-113">**MyContextModelSnapshot.cs**--A snapshot of your current model.</span></span> <span data-ttu-id="27989-114">Se usa para determinar qué ha cambiado al agregar la siguiente migración.</span><span class="sxs-lookup"><span data-stu-id="27989-114">Used to determine what changed when adding the next migration.</span></span>

<span data-ttu-id="27989-115">La marca de tiempo del nombre del archivo ayuda a mantenerlos ordenados cronológicamente para que se pueda ver la progresión de cambios.</span><span class="sxs-lookup"><span data-stu-id="27989-115">The timestamp in the filename helps keep them ordered chronologically so you can see the progression of changes.</span></span>

> [!TIP]
> <span data-ttu-id="27989-116">Puede mover los archivos de Migraciones y cambiar su espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="27989-116">You are free to move Migrations files and change their namespace.</span></span> <span data-ttu-id="27989-117">Se crean nuevas migraciones como elementos del mismo nivel de la última migración.</span><span class="sxs-lookup"><span data-stu-id="27989-117">New migrations are created as siblings of the last migration.</span></span>

<span data-ttu-id="27989-118">Luego aplique la migración a la base de datos para crear el esquema.</span><span class="sxs-lookup"><span data-stu-id="27989-118">Next, apply the migration to the database to create the schema.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="adding-another-migration"></a><span data-ttu-id="27989-119">Adición de otra migración</span><span class="sxs-lookup"><span data-stu-id="27989-119">Adding another migration</span></span>
------------------------
<span data-ttu-id="27989-120">Después de realizar cambios en el modelo de EF Core, el esquema de la base de datos no está sincronizado. Para ponerlo al día, agregue otra migración.</span><span class="sxs-lookup"><span data-stu-id="27989-120">After making changes to your EF Core model, the database schema will be out of sync. To bring it up to date, add another migration.</span></span> <span data-ttu-id="27989-121">El nombre de la migración se puede usar como mensaje de confirmación en un sistema de control de versiones.</span><span class="sxs-lookup"><span data-stu-id="27989-121">The migration name can be used like a commit message in a version control system.</span></span> <span data-ttu-id="27989-122">Por ejemplo, si se han realizado cambios para guardar las opiniones de clientes sobre productos, podría elegir algo parecido a *AddProductReviews*.</span><span class="sxs-lookup"><span data-stu-id="27989-122">For example, if I made changes to save customer reviews of products, I might choose something like *AddProductReviews*.</span></span>

``` powershell
Add-Migration AddProductReviews
```
``` Console
dotnet ef migrations add AddProductReviews
```

<span data-ttu-id="27989-123">Una vez que se ha aplicado scaffolding a la migración, debe revisar su precisión y agregar cualquier operación adicional necesaria para aplicarla correctamente.</span><span class="sxs-lookup"><span data-stu-id="27989-123">Once the migration is scaffolded, you should review it for accuracy and add any additional operations required to apply it correctly.</span></span> <span data-ttu-id="27989-124">Por ejemplo, la migración podría contener las siguientes operaciones:</span><span class="sxs-lookup"><span data-stu-id="27989-124">For example, your migration might contain the following operations:</span></span>

``` csharp
migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");

migrationBuilder.AddColumn<string>(
    name: "Name",
    table: "Customer",
    nullable: true);
```

<span data-ttu-id="27989-125">Aunque estas operaciones hacen que el esquema de la base de datos sea compatible, no conservan los nombres de cliente existentes.</span><span class="sxs-lookup"><span data-stu-id="27989-125">While these operations make the database schema compatible, they don't preserve the existing customer names.</span></span> <span data-ttu-id="27989-126">Para que sea mejor, vuelva a escribirla como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="27989-126">To make it better, rewrite it as follows.</span></span>

``` csharp
migrationBuilder.AddColumn<string>(
    name: "Name",
    table: "Customer",
    nullable: true);

migrationBuilder.Sql(
@"
    UPDATE Customer
    SET Name = FirstName + ' ' + LastName;
");

migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");
```

> [!TIP]
> <span data-ttu-id="27989-127">Al agregar una nueva migración, se advierte si se ha aplicado scaffolding a una operación que pueda dar lugar a pérdida de datos (por ejemplo, borrado de una columna).</span><span class="sxs-lookup"><span data-stu-id="27989-127">Adding a new migration warns when an operation is scaffolded that may result in data loss (like dropping a column).</span></span> <span data-ttu-id="27989-128">Asegúrese de revisar cuidadosamente la precisión de estas migraciones.</span><span class="sxs-lookup"><span data-stu-id="27989-128">Be sure to especially review these migrations for accuracy.</span></span>

<span data-ttu-id="27989-129">Aplique la migración a la base de datos con el comando apropiado.</span><span class="sxs-lookup"><span data-stu-id="27989-129">Apply the migration to the database using the appropriate command.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="removing-a-migration"></a><span data-ttu-id="27989-130">Eliminación de una migración</span><span class="sxs-lookup"><span data-stu-id="27989-130">Removing a migration</span></span>
--------------------
<span data-ttu-id="27989-131">A veces uno agrega una migración y se da cuenta de que debe realizar cambios adicionales en el modelo de EF Core antes de aplicarla.</span><span class="sxs-lookup"><span data-stu-id="27989-131">Sometimes you add a migration and realize you need to make additional changes to your EF Core model before applying it.</span></span>
<span data-ttu-id="27989-132">Para quitar la última migración, use este comando.</span><span class="sxs-lookup"><span data-stu-id="27989-132">To remove the last migration, use this command.</span></span>

``` powershell
Remove-Migration
```
``` Console
dotnet ef migrations remove
```

<span data-ttu-id="27989-133">Después de quitarla, puede realizar los cambios de modelo adicionales y volver a agregarla.</span><span class="sxs-lookup"><span data-stu-id="27989-133">After removing it, you can make the additional model changes and add it again.</span></span>

<a name="reverting-a-migration"></a><span data-ttu-id="27989-134">Reversión de una migración</span><span class="sxs-lookup"><span data-stu-id="27989-134">Reverting a migration</span></span>
---------------------
<span data-ttu-id="27989-135">Si ya ha aplicado una migración (o varias migraciones) a la base de datos pero necesita revertirla, puede usar el mismo comando que para aplicar migraciones, aunque debe especificar el nombre de la migración que quiere revertir.</span><span class="sxs-lookup"><span data-stu-id="27989-135">If you already applied a migration (or several migrations) to the database but need to revert it, you can use the same command to apply migrations, but specify the name of the migration you want to roll back to.</span></span>

``` powershell
Update-Database LastGoodMigration
```
``` Console
dotnet ef database update LastGoodMigration
```

<a name="empty-migrations"></a><span data-ttu-id="27989-136">Migraciones vacías</span><span class="sxs-lookup"><span data-stu-id="27989-136">Empty migrations</span></span>
----------------
<span data-ttu-id="27989-137">A veces resulta útil agregar una migración sin realizar ningún cambio de modelo.</span><span class="sxs-lookup"><span data-stu-id="27989-137">Sometimes it's useful to add a migration without making any model changes.</span></span> <span data-ttu-id="27989-138">En este caso, al agregar una nueva migración se crea una vacía.</span><span class="sxs-lookup"><span data-stu-id="27989-138">In this case, adding a new migration creates an empty one.</span></span> <span data-ttu-id="27989-139">Puede personalizar esta migración para llevar a cabo operaciones que no estén directamente relacionadas con el modelo de EF Core.</span><span class="sxs-lookup"><span data-stu-id="27989-139">You can customize this migration to perform operations that don't directly relate to the EF Core model.</span></span>
<span data-ttu-id="27989-140">Algunos aspectos que podría querer administrar de esta manera son:</span><span class="sxs-lookup"><span data-stu-id="27989-140">Some things you might want to manage this way are:</span></span>

* <span data-ttu-id="27989-141">Búsqueda de texto completo</span><span class="sxs-lookup"><span data-stu-id="27989-141">Full-Text Search</span></span>
* <span data-ttu-id="27989-142">Funciones</span><span class="sxs-lookup"><span data-stu-id="27989-142">Functions</span></span>
* <span data-ttu-id="27989-143">Procedimientos almacenados</span><span class="sxs-lookup"><span data-stu-id="27989-143">Stored procedures</span></span>
* <span data-ttu-id="27989-144">Desencadenadores</span><span class="sxs-lookup"><span data-stu-id="27989-144">Triggers</span></span>
* <span data-ttu-id="27989-145">Vistas</span><span class="sxs-lookup"><span data-stu-id="27989-145">Views</span></span>
* <span data-ttu-id="27989-146">Etc.</span><span class="sxs-lookup"><span data-stu-id="27989-146">etc.</span></span>

<a name="generating-a-sql-script"></a><span data-ttu-id="27989-147">Generación de un script SQL</span><span class="sxs-lookup"><span data-stu-id="27989-147">Generating a SQL script</span></span>
-----------------------
<span data-ttu-id="27989-148">Al depurar las migraciones o implementarlas en una base de datos de producción, es útil generar un script SQL.</span><span class="sxs-lookup"><span data-stu-id="27989-148">When debugging your migrations or deploying them to a production database, it's useful to generate a SQL script.</span></span> <span data-ttu-id="27989-149">El script luego se puede revisar y ajustar a las necesidades de una base de datos de producción.</span><span class="sxs-lookup"><span data-stu-id="27989-149">The script can then be further reviewed for accuracy and tuned to fit the needs of a production database.</span></span> <span data-ttu-id="27989-150">El script también puede usarse junto con una tecnología de implementación.</span><span class="sxs-lookup"><span data-stu-id="27989-150">The script can also be used in conjunction with a deployment technology.</span></span> <span data-ttu-id="27989-151">El comando básico es el siguiente.</span><span class="sxs-lookup"><span data-stu-id="27989-151">The basic command is as follows.</span></span>

``` powershell
Script-Migration
```
``` Console
dotnet ef migrations script
```

<span data-ttu-id="27989-152">Hay varias opciones para este comando.</span><span class="sxs-lookup"><span data-stu-id="27989-152">There are several options to this command.</span></span>

<span data-ttu-id="27989-153">La migración **from** debe ser la última migración aplicada a la base de datos antes de ejecutar el script.</span><span class="sxs-lookup"><span data-stu-id="27989-153">The **from** migration should be the last migration applied to the database before running the script.</span></span> <span data-ttu-id="27989-154">Si no se han aplicado migraciones, especifique `0` (es el valor predeterminado).</span><span class="sxs-lookup"><span data-stu-id="27989-154">If no migrations have been applied, specify `0` (this is the default).</span></span>

<span data-ttu-id="27989-155">La migración **to** debe ser la última migración que se va a aplicar a la base de datos después de ejecutar el script.</span><span class="sxs-lookup"><span data-stu-id="27989-155">The **to** migration is the last migration that will be applied to the database after running the script.</span></span> <span data-ttu-id="27989-156">El valor predeterminado es la última migración del proyecto.</span><span class="sxs-lookup"><span data-stu-id="27989-156">This defaults to the last migration in your project.</span></span>

<span data-ttu-id="27989-157">Se puede generar un script **idempotent** de forma opcional.</span><span class="sxs-lookup"><span data-stu-id="27989-157">An **idempotent** script can optionally be generated.</span></span> <span data-ttu-id="27989-158">Este script solo aplica migraciones si aún no se han aplicado a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="27989-158">This script only applies migrations if they haven't already been applied to the database.</span></span> <span data-ttu-id="27989-159">Es útil si no sabe exactamente cuál ha sido la última migración aplicada a la base de datos o si va a implementar en varias bases de datos que pueden encontrarse en migraciones diferentes.</span><span class="sxs-lookup"><span data-stu-id="27989-159">This is useful if you don't exactly know what the last migration applied to the database was or if you are deploying to multiple databases that may each be at a different migration.</span></span>

<a name="applying-migrations-at-runtime"></a><span data-ttu-id="27989-160">Aplicación de migraciones en tiempo de ejecución</span><span class="sxs-lookup"><span data-stu-id="27989-160">Applying migrations at runtime</span></span>
------------------------------
<span data-ttu-id="27989-161">Algunas aplicaciones pueden querer aplicar migraciones en tiempo de ejecución durante el inicio o la primera ejecución.</span><span class="sxs-lookup"><span data-stu-id="27989-161">Some apps may want to apply migrations at runtime during startup or first run.</span></span> <span data-ttu-id="27989-162">Para ello, se usa el método `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="27989-162">Do this using the `Migrate()` method.</span></span>

<span data-ttu-id="27989-163">Precaución, ya que este método no es para todos.</span><span class="sxs-lookup"><span data-stu-id="27989-163">Caution, this approach isn't for everyone.</span></span> <span data-ttu-id="27989-164">Aunque es excelente para las aplicaciones con una base de datos local, la mayoría de las aplicaciones necesitan estrategias de implementación más sólidas, como la generación de scripts SQL.</span><span class="sxs-lookup"><span data-stu-id="27989-164">While it's great for apps with a local database, most applications will require more robust deployment strategy like generating SQL scripts.</span></span>

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
> <span data-ttu-id="27989-165">No llame a `EnsureCreated()` antes de `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="27989-165">Don't call `EnsureCreated()` before `Migrate()`.</span></span> <span data-ttu-id="27989-166">`EnsureCreated()` omite las migraciones para crear el esquema, lo cual provoca un error de `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="27989-166">`EnsureCreated()` bypasses Migrations to create the schema, which causes `Migrate()` to fail.</span></span>

> [!NOTE]
> <span data-ttu-id="27989-167">Este método se basa en el servicio `IMigrator`, que se puede usar para escenarios más avanzados.</span><span class="sxs-lookup"><span data-stu-id="27989-167">This method builds on top of the `IMigrator` service, which can be used for more advanced scenarios.</span></span> <span data-ttu-id="27989-168">Use `DbContext.GetService<IMigrator>()` para acceder a él.</span><span class="sxs-lookup"><span data-stu-id="27989-168">Use `DbContext.GetService<IMigrator>()` to access it.</span></span>


  [1]: ../../modeling/index.md
  [2]: ../../miscellaneous/cli/index.md
