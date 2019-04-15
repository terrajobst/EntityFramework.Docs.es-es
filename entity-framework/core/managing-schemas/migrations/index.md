---
title: Migraciones - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/05/2018
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: b94ac567644a9d98a05a40857cc072c500203370
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562564"
---
<a name="migrations"></a><span data-ttu-id="39fb3-102">Migraciones</span><span class="sxs-lookup"><span data-stu-id="39fb3-102">Migrations</span></span>
==========

<span data-ttu-id="39fb3-103">Un modelo de datos cambia durante el desarrollo y deja de estar sincronizado con la base de datos.</span><span class="sxs-lookup"><span data-stu-id="39fb3-103">A data model changes during development and gets out of sync with the database.</span></span> <span data-ttu-id="39fb3-104">Puede quitar la base de datos y dejar que EF cree una que coincida con el modelo, pero este procedimiento provoca la pérdida de datos.</span><span class="sxs-lookup"><span data-stu-id="39fb3-104">You can drop the database and let EF create a new one that matches the model, but this procedure results in the loss of data.</span></span> <span data-ttu-id="39fb3-105">La característica de migraciones de EF Core proporciona una manera de actualizar incrementalmente el esquema de la base de datos para mantenerla sincronizada con el modelo de datos de la aplicación al tiempo que se conservan los datos existentes en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="39fb3-105">The migrations feature in EF Core provides a way to incrementally update the database schema to keep it in sync with the application's data model while preserving existing data in the database.</span></span>

<span data-ttu-id="39fb3-106">Las migraciones incluyen herramientas de línea de comandos y API que facilitan las siguientes tareas:</span><span class="sxs-lookup"><span data-stu-id="39fb3-106">Migrations includes command-line tools and APIs that help with the following tasks:</span></span>

* <span data-ttu-id="39fb3-107">[Crear una migración](#create-a-migration).</span><span class="sxs-lookup"><span data-stu-id="39fb3-107">[Create a migration](#create-a-migration).</span></span> <span data-ttu-id="39fb3-108">Generar código que puede actualizar la base de datos para sincronizarla con un conjunto de cambios en el modelo.</span><span class="sxs-lookup"><span data-stu-id="39fb3-108">Generate code that can update the database to sync it with a set of model changes.</span></span>
* <span data-ttu-id="39fb3-109">[Actualizar la base de datos](#update-the-database).</span><span class="sxs-lookup"><span data-stu-id="39fb3-109">[Update the database](#update-the-database).</span></span> <span data-ttu-id="39fb3-110">Aplicar las migraciones pendientes para actualizar el esquema de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="39fb3-110">Apply pending migrations to update the database schema.</span></span>
* <span data-ttu-id="39fb3-111">[Personalizar el código de migración](#customize-migration-code).</span><span class="sxs-lookup"><span data-stu-id="39fb3-111">[Customize migration code](#customize-migration-code).</span></span> <span data-ttu-id="39fb3-112">A veces el código generado debe modificarse o complementarse.</span><span class="sxs-lookup"><span data-stu-id="39fb3-112">Sometimes the generated code needs to be modified or supplemented.</span></span>
* <span data-ttu-id="39fb3-113">[Quitar una migración](#remove-a-migration).</span><span class="sxs-lookup"><span data-stu-id="39fb3-113">[Remove a migration](#remove-a-migration).</span></span> <span data-ttu-id="39fb3-114">Eliminar el código generado.</span><span class="sxs-lookup"><span data-stu-id="39fb3-114">Delete the generated code.</span></span>
* <span data-ttu-id="39fb3-115">[Revertir una migración](#revert-a-migration).</span><span class="sxs-lookup"><span data-stu-id="39fb3-115">[Revert a migration](#revert-a-migration).</span></span> <span data-ttu-id="39fb3-116">Deshacer los cambios de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="39fb3-116">Undo the database changes.</span></span>
* <span data-ttu-id="39fb3-117">[Generar scripts SQL](#generate-sql-scripts).</span><span class="sxs-lookup"><span data-stu-id="39fb3-117">[Generate SQL scripts](#generate-sql-scripts).</span></span> <span data-ttu-id="39fb3-118">Puede que necesite un script para actualizar una base de datos de producción o para solucionar problemas con el código de migración.</span><span class="sxs-lookup"><span data-stu-id="39fb3-118">You might need a script to update a production database or to troubleshoot migration code.</span></span>
* <span data-ttu-id="39fb3-119">[Aplicar migraciones en tiempo de ejecución](#apply-migrations-at-runtime).</span><span class="sxs-lookup"><span data-stu-id="39fb3-119">[Apply migrations at runtime](#apply-migrations-at-runtime).</span></span> <span data-ttu-id="39fb3-120">Si las actualizaciones en tiempo de diseño y la ejecución de scripts no son las mejores opciones, llame al método `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="39fb3-120">When design-time updates and running scripts aren't the best options, call the `Migrate()` method.</span></span>

<a name="install-the-tools"></a><span data-ttu-id="39fb3-121">Instalar las herramientas</span><span class="sxs-lookup"><span data-stu-id="39fb3-121">Install the tools</span></span>
-----------------

<span data-ttu-id="39fb3-122">Instale las [herramientas de línea de comandos](xref:core/miscellaneous/cli/index):</span><span class="sxs-lookup"><span data-stu-id="39fb3-122">Install the [command-line tools](xref:core/miscellaneous/cli/index):</span></span>
* <span data-ttu-id="39fb3-123">Para Visual Studio, se recomiendan las [herramientas de la Consola del Administrador de paquetes](xref:core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="39fb3-123">For Visual Studio, we recommend the [Package Manager Console tools](xref:core/miscellaneous/cli/powershell).</span></span>
* <span data-ttu-id="39fb3-124">Para otros entornos de desarrollo, elija las [herramientas de la CLI de .NET Core](xref:core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="39fb3-124">For other development environments, choose the [.NET Core CLI tools](xref:core/miscellaneous/cli/dotnet).</span></span>

<a name="create-a-migration"></a><span data-ttu-id="39fb3-125">Crear una migración</span><span class="sxs-lookup"><span data-stu-id="39fb3-125">Create a migration</span></span>
------------------

<span data-ttu-id="39fb3-126">Una vez [definido el modelo inicial](xref:core/modeling/index), es el momento de crear la base de datos.</span><span class="sxs-lookup"><span data-stu-id="39fb3-126">After you've [defined your initial model](xref:core/modeling/index), it's time to create the database.</span></span> <span data-ttu-id="39fb3-127">Para agregar una migración inicial, ejecute el siguiente comando.</span><span class="sxs-lookup"><span data-stu-id="39fb3-127">To add an initial migration, run the following command.</span></span>

``` powershell
Add-Migration InitialCreate
```
``` Console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="39fb3-128">Se agregan tres archivos al proyecto en el directorio **Migraciones**:</span><span class="sxs-lookup"><span data-stu-id="39fb3-128">Three files are added to your project under the **Migrations** directory:</span></span>

* <span data-ttu-id="39fb3-129">**XXXXXXXXXXXXXX_InitialCreate.cs**: archivo principal de las migraciones.</span><span class="sxs-lookup"><span data-stu-id="39fb3-129">**XXXXXXXXXXXXXX_InitialCreate.cs**--The main migrations file.</span></span> <span data-ttu-id="39fb3-130">Contiene las operaciones necesarias para aplicar la migración (en `Up()`) y para revertirla (en `Down()`).</span><span class="sxs-lookup"><span data-stu-id="39fb3-130">Contains the operations necessary to apply the migration (in `Up()`) and to revert it (in `Down()`).</span></span>
* <span data-ttu-id="39fb3-131">**XXXXXXXXXXXXXX_InitialCreate.Designer.cs**: archivo de metadatos de las migraciones.</span><span class="sxs-lookup"><span data-stu-id="39fb3-131">**XXXXXXXXXXXXXX_InitialCreate.Designer.cs**--The migrations metadata file.</span></span> <span data-ttu-id="39fb3-132">Contiene información que usa EF.</span><span class="sxs-lookup"><span data-stu-id="39fb3-132">Contains information used by EF.</span></span>
* <span data-ttu-id="39fb3-133">**MyContextModelSnapshot.cs**: instantánea del modelo actual.</span><span class="sxs-lookup"><span data-stu-id="39fb3-133">**MyContextModelSnapshot.cs**--A snapshot of your current model.</span></span> <span data-ttu-id="39fb3-134">Se usa para determinar qué ha cambiado al agregar la siguiente migración.</span><span class="sxs-lookup"><span data-stu-id="39fb3-134">Used to determine what changed when adding the next migration.</span></span>

<span data-ttu-id="39fb3-135">La marca de tiempo del nombre del archivo ayuda a mantenerlos ordenados cronológicamente para que se pueda ver la progresión de cambios.</span><span class="sxs-lookup"><span data-stu-id="39fb3-135">The timestamp in the filename helps keep them ordered chronologically so you can see the progression of changes.</span></span>

> [!TIP]
> <span data-ttu-id="39fb3-136">Puede mover los archivos de Migraciones y cambiar su espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="39fb3-136">You are free to move Migrations files and change their namespace.</span></span> <span data-ttu-id="39fb3-137">Se crean nuevas migraciones como elementos del mismo nivel de la última migración.</span><span class="sxs-lookup"><span data-stu-id="39fb3-137">New migrations are created as siblings of the last migration.</span></span>

<a name="update-the-database"></a><span data-ttu-id="39fb3-138">Actualizar la base de datos</span><span class="sxs-lookup"><span data-stu-id="39fb3-138">Update the database</span></span>
-------------------

<span data-ttu-id="39fb3-139">Luego aplique la migración a la base de datos para crear el esquema.</span><span class="sxs-lookup"><span data-stu-id="39fb3-139">Next, apply the migration to the database to create the schema.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="customize-migration-code"></a><span data-ttu-id="39fb3-140">Personalizar el código de migración</span><span class="sxs-lookup"><span data-stu-id="39fb3-140">Customize migration code</span></span>
------------------------

<span data-ttu-id="39fb3-141">Después de realizar cambios en el modelo de EF Core, puede que el esquema de la base de datos no esté sincronizado. Para ponerlo al día, agregue otra migración.</span><span class="sxs-lookup"><span data-stu-id="39fb3-141">After making changes to your EF Core model, the database schema might be out of sync. To bring it up to date, add another migration.</span></span> <span data-ttu-id="39fb3-142">El nombre de la migración se puede usar como mensaje de confirmación en un sistema de control de versiones.</span><span class="sxs-lookup"><span data-stu-id="39fb3-142">The migration name can be used like a commit message in a version control system.</span></span> <span data-ttu-id="39fb3-143">Por ejemplo, podría elegir un nombre como *AddProductReviews* si el cambio es una nueva clase de entidad para las revisiones.</span><span class="sxs-lookup"><span data-stu-id="39fb3-143">For example, you might choose a name like *AddProductReviews* if the change is a new entity class for reviews.</span></span>

``` powershell
Add-Migration AddProductReviews
```
``` Console
dotnet ef migrations add AddProductReviews
```

<span data-ttu-id="39fb3-144">Tras aplicar scaffolding a la migración (código generado para ella), revise el código para mayor precisión y agregue, quite o modifique todas las operaciones necesarias para aplicarla correctamente.</span><span class="sxs-lookup"><span data-stu-id="39fb3-144">Once the migration is scaffolded (code generated for it), review the code for accuracy and add, remove or modify any operations required to apply it correctly.</span></span>

<span data-ttu-id="39fb3-145">Por ejemplo, una migración podría contener las siguientes operaciones:</span><span class="sxs-lookup"><span data-stu-id="39fb3-145">For example, a migration might contain the following operations:</span></span>

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

<span data-ttu-id="39fb3-146">Aunque estas operaciones hacen que el esquema de la base de datos sea compatible, no conservan los nombres de cliente existentes.</span><span class="sxs-lookup"><span data-stu-id="39fb3-146">While these operations make the database schema compatible, they don't preserve the existing customer names.</span></span> <span data-ttu-id="39fb3-147">Para que sea mejor, vuelva a escribirla como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="39fb3-147">To make it better, rewrite it as follows.</span></span>

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
> <span data-ttu-id="39fb3-148">El proceso de scaffolding de la migración advierte si una operación puede ocasionar una pérdida de datos (como el borrado de una columna).</span><span class="sxs-lookup"><span data-stu-id="39fb3-148">The migration scaffolding process warns when an operation might result in data loss (like dropping a column).</span></span> <span data-ttu-id="39fb3-149">Si aparece dicha advertencia, asegúrese especialmente de revisar el código de las migraciones para mayor precisión.</span><span class="sxs-lookup"><span data-stu-id="39fb3-149">If you see that warning, be especially sure to review the migrations code for accuracy.</span></span>

<span data-ttu-id="39fb3-150">Aplique la migración a la base de datos con el comando apropiado.</span><span class="sxs-lookup"><span data-stu-id="39fb3-150">Apply the migration to the database using the appropriate command.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

### <a name="empty-migrations"></a><span data-ttu-id="39fb3-151">Migraciones vacías</span><span class="sxs-lookup"><span data-stu-id="39fb3-151">Empty migrations</span></span>

<span data-ttu-id="39fb3-152">A veces resulta útil agregar una migración sin realizar ningún cambio de modelo.</span><span class="sxs-lookup"><span data-stu-id="39fb3-152">Sometimes it's useful to add a migration without making any model changes.</span></span> <span data-ttu-id="39fb3-153">En este caso, agregar una nueva migración crea archivos de código con clases vacías.</span><span class="sxs-lookup"><span data-stu-id="39fb3-153">In this case, adding a new migration creates code files with empty classes.</span></span> <span data-ttu-id="39fb3-154">Puede personalizar esta migración para llevar a cabo operaciones que no estén directamente relacionadas con el modelo de EF Core.</span><span class="sxs-lookup"><span data-stu-id="39fb3-154">You can customize this migration to perform operations that don't directly relate to the EF Core model.</span></span> <span data-ttu-id="39fb3-155">Algunos aspectos que podría querer administrar de esta manera son:</span><span class="sxs-lookup"><span data-stu-id="39fb3-155">Some things you might want to manage this way are:</span></span>

* <span data-ttu-id="39fb3-156">Búsqueda de texto completo</span><span class="sxs-lookup"><span data-stu-id="39fb3-156">Full-Text Search</span></span>
* <span data-ttu-id="39fb3-157">Funciones</span><span class="sxs-lookup"><span data-stu-id="39fb3-157">Functions</span></span>
* <span data-ttu-id="39fb3-158">Procedimientos almacenados</span><span class="sxs-lookup"><span data-stu-id="39fb3-158">Stored procedures</span></span>
* <span data-ttu-id="39fb3-159">Desencadenadores</span><span class="sxs-lookup"><span data-stu-id="39fb3-159">Triggers</span></span>
* <span data-ttu-id="39fb3-160">Vistas</span><span class="sxs-lookup"><span data-stu-id="39fb3-160">Views</span></span>

<a name="remove-a-migration"></a><span data-ttu-id="39fb3-161">Quitar una migración</span><span class="sxs-lookup"><span data-stu-id="39fb3-161">Remove a migration</span></span>
------------------
<span data-ttu-id="39fb3-162">A veces uno agrega una migración y se da cuenta de que debe realizar cambios adicionales en el modelo de EF Core antes de aplicarla.</span><span class="sxs-lookup"><span data-stu-id="39fb3-162">Sometimes you add a migration and realize you need to make additional changes to your EF Core model before applying it.</span></span> <span data-ttu-id="39fb3-163">Para quitar la última migración, use este comando.</span><span class="sxs-lookup"><span data-stu-id="39fb3-163">To remove the last migration, use this command.</span></span>

``` powershell
Remove-Migration
```
``` Console
dotnet ef migrations remove
```

<span data-ttu-id="39fb3-164">Después de quitar la migración, puede realizar los cambios de modelo adicionales y volver a agregarla.</span><span class="sxs-lookup"><span data-stu-id="39fb3-164">After removing the migration, you can make the additional model changes and add it again.</span></span>

<a name="revert-a-migration"></a><span data-ttu-id="39fb3-165">Revertir una migración</span><span class="sxs-lookup"><span data-stu-id="39fb3-165">Revert a migration</span></span>
------------------
<span data-ttu-id="39fb3-166">Si ya ha aplicado una migración (o varias migraciones) a la base de datos pero necesita revertirla, puede usar el mismo comando que para aplicar migraciones, aunque debe especificar el nombre de la migración que quiere revertir.</span><span class="sxs-lookup"><span data-stu-id="39fb3-166">If you already applied a migration (or several migrations) to the database but need to revert it, you can use the same command to apply migrations, but specify the name of the migration you want to roll back to.</span></span>

``` powershell
Update-Database LastGoodMigration
```
``` Console
dotnet ef database update LastGoodMigration
```

<a name="generate-sql-scripts"></a><span data-ttu-id="39fb3-167">Generar scripts SQL</span><span class="sxs-lookup"><span data-stu-id="39fb3-167">Generate SQL scripts</span></span>
--------------------
<span data-ttu-id="39fb3-168">Al depurar las migraciones o implementarlas en una base de datos de producción, es útil generar un script SQL.</span><span class="sxs-lookup"><span data-stu-id="39fb3-168">When debugging your migrations or deploying them to a production database, it's useful to generate a SQL script.</span></span> <span data-ttu-id="39fb3-169">El script luego se puede revisar y ajustar a las necesidades de una base de datos de producción.</span><span class="sxs-lookup"><span data-stu-id="39fb3-169">The script can then be further reviewed for accuracy and tuned to fit the needs of a production database.</span></span> <span data-ttu-id="39fb3-170">El script también puede usarse junto con una tecnología de implementación.</span><span class="sxs-lookup"><span data-stu-id="39fb3-170">The script can also be used in conjunction with a deployment technology.</span></span> <span data-ttu-id="39fb3-171">El comando básico es el siguiente.</span><span class="sxs-lookup"><span data-stu-id="39fb3-171">The basic command is as follows.</span></span>

``` powershell
Script-Migration
```
``` Console
dotnet ef migrations script
```

<span data-ttu-id="39fb3-172">Hay varias opciones para este comando.</span><span class="sxs-lookup"><span data-stu-id="39fb3-172">There are several options to this command.</span></span>

<span data-ttu-id="39fb3-173">La migración **from** debe ser la última migración aplicada a la base de datos antes de ejecutar el script.</span><span class="sxs-lookup"><span data-stu-id="39fb3-173">The **from** migration should be the last migration applied to the database before running the script.</span></span> <span data-ttu-id="39fb3-174">Si no se han aplicado migraciones, especifique `0` (es el valor predeterminado).</span><span class="sxs-lookup"><span data-stu-id="39fb3-174">If no migrations have been applied, specify `0` (this is the default).</span></span>

<span data-ttu-id="39fb3-175">La migración **to** debe ser la última migración que se va a aplicar a la base de datos después de ejecutar el script.</span><span class="sxs-lookup"><span data-stu-id="39fb3-175">The **to** migration is the last migration that will be applied to the database after running the script.</span></span> <span data-ttu-id="39fb3-176">El valor predeterminado es la última migración del proyecto.</span><span class="sxs-lookup"><span data-stu-id="39fb3-176">This defaults to the last migration in your project.</span></span>

<span data-ttu-id="39fb3-177">Se puede generar un script **idempotent** de forma opcional.</span><span class="sxs-lookup"><span data-stu-id="39fb3-177">An **idempotent** script can optionally be generated.</span></span> <span data-ttu-id="39fb3-178">Este script solo aplica migraciones si aún no se han aplicado a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="39fb3-178">This script only applies migrations if they haven't already been applied to the database.</span></span> <span data-ttu-id="39fb3-179">Es útil si no sabe exactamente cuál ha sido la última migración aplicada a la base de datos o si va a implementar en varias bases de datos que pueden encontrarse en migraciones diferentes.</span><span class="sxs-lookup"><span data-stu-id="39fb3-179">This is useful if you don't exactly know what the last migration applied to the database was or if you are deploying to multiple databases that may each be at a different migration.</span></span>

<a name="apply-migrations-at-runtime"></a><span data-ttu-id="39fb3-180">Aplicar migraciones en tiempo de ejecución</span><span class="sxs-lookup"><span data-stu-id="39fb3-180">Apply migrations at runtime</span></span>
---------------------------
<span data-ttu-id="39fb3-181">Algunas aplicaciones pueden querer aplicar migraciones en tiempo de ejecución durante el inicio o la primera ejecución.</span><span class="sxs-lookup"><span data-stu-id="39fb3-181">Some apps may want to apply migrations at runtime during startup or first run.</span></span> <span data-ttu-id="39fb3-182">Para ello, se usa el método `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="39fb3-182">Do this using the `Migrate()` method.</span></span>

<span data-ttu-id="39fb3-183">Este método se basa en el servicio `IMigrator`, que se puede usar para escenarios más avanzados.</span><span class="sxs-lookup"><span data-stu-id="39fb3-183">This method builds on top of the `IMigrator` service, which can be used for more advanced scenarios.</span></span> <span data-ttu-id="39fb3-184">Use `DbContext.GetService<IMigrator>()` para acceder a él.</span><span class="sxs-lookup"><span data-stu-id="39fb3-184">Use `DbContext.GetService<IMigrator>()` to access it.</span></span>

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
> * <span data-ttu-id="39fb3-185">Este método no es para todos.</span><span class="sxs-lookup"><span data-stu-id="39fb3-185">This approach isn't for everyone.</span></span> <span data-ttu-id="39fb3-186">Aunque es excelente para las aplicaciones con una base de datos local, la mayoría de las aplicaciones necesitan estrategias de implementación más sólidas, como la generación de scripts SQL.</span><span class="sxs-lookup"><span data-stu-id="39fb3-186">While it's great for apps with a local database, most applications will require more robust deployment strategy like generating SQL scripts.</span></span>
> * <span data-ttu-id="39fb3-187">No llame a `EnsureCreated()` antes de `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="39fb3-187">Don't call `EnsureCreated()` before `Migrate()`.</span></span> <span data-ttu-id="39fb3-188">`EnsureCreated()` omite las migraciones para crear el esquema, lo cual provoca un error de `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="39fb3-188">`EnsureCreated()` bypasses Migrations to create the schema, which causes `Migrate()` to fail.</span></span>

<a name="next-steps"></a><span data-ttu-id="39fb3-189">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="39fb3-189">Next steps</span></span>
----------

<span data-ttu-id="39fb3-190">Para obtener más información, vea <xref:core/miscellaneous/cli/index>.</span><span class="sxs-lookup"><span data-stu-id="39fb3-190">For more information, see <xref:core/miscellaneous/cli/index>.</span></span>
