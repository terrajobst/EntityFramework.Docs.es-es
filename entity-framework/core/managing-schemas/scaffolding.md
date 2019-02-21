---
title: Reverse Engineering - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/13/2018
ms.assetid: 6263EF7D-4989-42E6-BDEE-45DA770342FB
uid: core/managing-schemas/scaffolding
ms.openlocfilehash: 6e61d2ebcf5ada365dcdb264bc371199574e12fa
ms.sourcegitcommit: 33b2e84dae96040f60a613186a24ff3c7b00b6db
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/21/2019
ms.locfileid: "56459190"
---
# <a name="reverse-engineering"></a><span data-ttu-id="8121c-102">Ingeniería inversa</span><span class="sxs-lookup"><span data-stu-id="8121c-102">Reverse Engineering</span></span>

<span data-ttu-id="8121c-103">Ingeniería inversa es el proceso de scaffolding de las clases de tipo de entidad y una clase DbContext según un esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="8121c-103">Reverse engineering is the process of scaffolding entity type classes and a DbContext class based on a database schema.</span></span> <span data-ttu-id="8121c-104">Puede realizarse mediante la `Scaffold-DbContext` comando de las herramientas de la consola de administrador de paquetes (PMC) de EF Core o la `dotnet ef dbcontext scaffold` comando de las herramientas de interfaz de línea de comandos (CLI) de .NET.</span><span class="sxs-lookup"><span data-stu-id="8121c-104">It can be performed using the `Scaffold-DbContext` command of the EF Core Package Manager Console (PMC) tools or the `dotnet ef dbcontext scaffold` command of the .NET Command-line Interface (CLI) tools.</span></span>

## <a name="installing"></a><span data-ttu-id="8121c-105">Instalación</span><span class="sxs-lookup"><span data-stu-id="8121c-105">Installing</span></span>

<span data-ttu-id="8121c-106">Antes de aplicar ingeniería inversa, deberá instalar el [herramientas PMC](xref:core/miscellaneous/cli/powershell) (solo Visual Studio) o el [herramientas CLI](xref:core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="8121c-106">Before reverse engineering, you'll need to install either the [PMC tools](xref:core/miscellaneous/cli/powershell) (Visual Studio only) or the [CLI tools](xref:core/miscellaneous/cli/dotnet).</span></span> <span data-ttu-id="8121c-107">Consulte los vínculos para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="8121c-107">See links for details.</span></span>

<span data-ttu-id="8121c-108">También deberá instalar una adecuada [proveedor de base de datos](xref:core/providers/index) para el esquema de base de datos que desea aplicar ingeniería inversa.</span><span class="sxs-lookup"><span data-stu-id="8121c-108">You'll also need to install an appropriate [database provider](xref:core/providers/index) for the database schema you want to reverse engineer.</span></span>

## <a name="connection-string"></a><span data-ttu-id="8121c-109">Cadena de conexión</span><span class="sxs-lookup"><span data-stu-id="8121c-109">Connection string</span></span>

<span data-ttu-id="8121c-110">El primer argumento para el comando es una cadena de conexión a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="8121c-110">The first argument to the command is a connection string to the database.</span></span> <span data-ttu-id="8121c-111">Las herramientas usará esta cadena de conexión para leer el esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="8121c-111">The tools will use this connection string to read the database schema.</span></span>

<span data-ttu-id="8121c-112">Cómo comillas y escape de la cadena de conexión depende de qué shell que esté utilizando para ejecutar el comando.</span><span class="sxs-lookup"><span data-stu-id="8121c-112">How you quote and escape the connection string depends on which shell you are using to execute the command.</span></span> <span data-ttu-id="8121c-113">Consulte la documentación de su shell para obtener información específica.</span><span class="sxs-lookup"><span data-stu-id="8121c-113">Refer to your shell's documentation for specifics.</span></span> <span data-ttu-id="8121c-114">Por ejemplo, PowerShell, deberá escape la `$` de caracteres, pero no `\`.</span><span class="sxs-lookup"><span data-stu-id="8121c-114">For example, PowerShell requires you to escape the `$` character, but not `\`.</span></span>

``` powershell
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer
```

``` Console
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="configuration-and-user-secrets"></a><span data-ttu-id="8121c-115">Configuración y los secretos de usuario</span><span class="sxs-lookup"><span data-stu-id="8121c-115">Configuration and User Secrets</span></span>

<span data-ttu-id="8121c-116">Si tiene un proyecto de ASP.NET Core, puede usar el `Name=<connection-string>` sintaxis para leer la cadena de conexión de configuración.</span><span class="sxs-lookup"><span data-stu-id="8121c-116">If you have an ASP.NET Core project, you can use the `Name=<connection-string>` syntax to read the connection string from configuration.</span></span>

<span data-ttu-id="8121c-117">Esto funciona bien con la [herramienta Secret Manager](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) separar la contraseña de la base de datos de la base de código.</span><span class="sxs-lookup"><span data-stu-id="8121c-117">This works well with the [Secret Manager tool](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) to keep your database password separate from your codebase.</span></span>

``` Console
dotnet user-secrets set ConnectionStrings.Chinook "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook"
dotnet ef dbcontext scaffold Name=Chinook Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="provider-name"></a><span data-ttu-id="8121c-118">Nombre del proveedor</span><span class="sxs-lookup"><span data-stu-id="8121c-118">Provider name</span></span>

<span data-ttu-id="8121c-119">El segundo argumento es el nombre del proveedor.</span><span class="sxs-lookup"><span data-stu-id="8121c-119">The second argument is the provider name.</span></span> <span data-ttu-id="8121c-120">El nombre del proveedor suele ser el mismo que el nombre del paquete NuGet del proveedor.</span><span class="sxs-lookup"><span data-stu-id="8121c-120">The provider name is typically the same as the provider's NuGet package name.</span></span>

## <a name="specifying-tables"></a><span data-ttu-id="8121c-121">Especificación de tablas</span><span class="sxs-lookup"><span data-stu-id="8121c-121">Specifying tables</span></span>

<span data-ttu-id="8121c-122">Todas las tablas en el esquema de base de datos son inversas diseñadas en tipos de entidad de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="8121c-122">All tables in the database schema are reverse engineered into entity types by default.</span></span> <span data-ttu-id="8121c-123">Puede limitar qué tablas son inversos ingeniería mediante la especificación de esquemas y tablas.</span><span class="sxs-lookup"><span data-stu-id="8121c-123">You can limit which tables are reverse engineered by specifying schemas and tables.</span></span>

<span data-ttu-id="8121c-124">El `-Schemas` parámetro en la PMC y `--schema` opción en la CLI puede utilizarse para incluir todas las tablas dentro de un esquema.</span><span class="sxs-lookup"><span data-stu-id="8121c-124">The `-Schemas` parameter in PMC and the `--schema` option in the CLI can be used to include every table within a schema.</span></span>

<span data-ttu-id="8121c-125">`-Tables` (PMC) y `--table` (CLI) puede usarse para incluir tablas específicas.</span><span class="sxs-lookup"><span data-stu-id="8121c-125">`-Tables` (PMC) and `--table` (CLI) can be used to include specific tables.</span></span>

<span data-ttu-id="8121c-126">Para incluir varias tablas en la PMC, use una matriz.</span><span class="sxs-lookup"><span data-stu-id="8121c-126">To include multiple tables in PMC, use an array.</span></span>

``` powershell
Scaffold-DbContext ... -Tables Artist, Album
```

<span data-ttu-id="8121c-127">Para incluir varias tablas en la CLI, especifique la opción varias veces.</span><span class="sxs-lookup"><span data-stu-id="8121c-127">To include multiple tables in the CLI, specify the option multiple times.</span></span>

``` Console
dotnet ef dbcontext scaffold ... --table Artist --table Album
```

## <a name="preserving-names"></a><span data-ttu-id="8121c-128">Conservación de nombres</span><span class="sxs-lookup"><span data-stu-id="8121c-128">Preserving names</span></span>

<span data-ttu-id="8121c-129">Los nombres de tabla y columna se corrigen que mejor coincida con las convenciones de nomenclatura .NET para los tipos y propiedades de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="8121c-129">Table and column names are fixed up to better match the .NET naming conventions for types and properties by default.</span></span> <span data-ttu-id="8121c-130">Especificar el `-UseDatabaseNames` cambie en PMC o el `--use-database-names` opción en la CLI, deshabilitará este comportamiento conserva los nombres de base de datos original tanto como sea posible.</span><span class="sxs-lookup"><span data-stu-id="8121c-130">Specifying the `-UseDatabaseNames` switch in PMC or the `--use-database-names` option in the CLI will disable this behavior preserving the original database names as much as possible.</span></span> <span data-ttu-id="8121c-131">Todavía se corregirán los identificadores no válidos de .NET y convenciones de nomenclatura .NET aún cumpla sintetizados nombres como propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="8121c-131">Invalid .NET identifiers will still be fixed and synthesized names like navigation properties will still conform to .NET naming conventions.</span></span>

## <a name="fluent-api-or-data-annotations"></a><span data-ttu-id="8121c-132">La API Fluent o las anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="8121c-132">Fluent API or Data Annotations</span></span>

<span data-ttu-id="8121c-133">Tipos de entidad se configuran mediante la API Fluent de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="8121c-133">Entity types are configured using the Fluent API by default.</span></span> <span data-ttu-id="8121c-134">Especificar `-DataAnnotations` (PMC) o `--data-annotations` (CLI) para usar en su lugar, las anotaciones de datos cuando sea posible.</span><span class="sxs-lookup"><span data-stu-id="8121c-134">Specify `-DataAnnotations` (PMC) or `--data-annotations` (CLI) to instead use data annotations when possible.</span></span>

<span data-ttu-id="8121c-135">Por ejemplo, mediante la API Fluent aplicará la técnica scaffolding esto:</span><span class="sxs-lookup"><span data-stu-id="8121c-135">For example, using the Fluent API will scaffold this:</span></span>

``` csharp
entity.Property(e => e.Title)
    .IsRequired()
    .HasMaxLength(160);
```

<span data-ttu-id="8121c-136">Al usar anotaciones de datos aplicará la técnica scaffolding esto:</span><span class="sxs-lookup"><span data-stu-id="8121c-136">While using Data Annotations will scaffold this:</span></span>

``` csharp
[Required]
[StringLength(160)]
public string Title { get; set; }
```

## <a name="dbcontext-name"></a><span data-ttu-id="8121c-137">Nombre de DbContext</span><span class="sxs-lookup"><span data-stu-id="8121c-137">DbContext name</span></span>

<span data-ttu-id="8121c-138">El nombre de clase con scaffolding de DbContext será el nombre de la base de datos con el sufijo *contexto* de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="8121c-138">The scaffolded DbContext class name will be the name of the database suffixed with *Context* by default.</span></span> <span data-ttu-id="8121c-139">Para especificar otro diferente, use `-Context` en PMC y `--context` en la CLI.</span><span class="sxs-lookup"><span data-stu-id="8121c-139">To specify a different one, use `-Context` in PMC and `--context` in the CLI.</span></span>

## <a name="directories-and-namespaces"></a><span data-ttu-id="8121c-140">Espacios de nombres y directorios</span><span class="sxs-lookup"><span data-stu-id="8121c-140">Directories and namespaces</span></span>

<span data-ttu-id="8121c-141">Las clases de entidad y una clase DbContext se ha aplicado scaffolding en el directorio raíz del proyecto y usan el espacio de nombres del proyecto predeterminado.</span><span class="sxs-lookup"><span data-stu-id="8121c-141">The entity classes and a DbContext class are scaffolded into the project's root directory and use the project's default namespace.</span></span> <span data-ttu-id="8121c-142">Puede especificar el directorio donde se ha aplicado scaffolding a las clases con `-OutputDir` (PMC) o `--output-dir` (CLI).</span><span class="sxs-lookup"><span data-stu-id="8121c-142">You can specify the directory where classes are scaffolded using `-OutputDir` (PMC) or `--output-dir` (CLI).</span></span> <span data-ttu-id="8121c-143">El espacio de nombres será el espacio de nombres raíz y los nombres de sus subdirectorios en el directorio raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="8121c-143">The namespace will be the root namespace plus the names of any subdirectories under the project's root directory.</span></span>

<span data-ttu-id="8121c-144">También puede usar `-ContextDir` (PMC) y `--context-dir` (CLI) para aplicar scaffolding a la clase DbContext en un directorio diferente de las clases de tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="8121c-144">You can also use `-ContextDir` (PMC) and `--context-dir` (CLI) to scaffold the DbContext class into a separate directory from the entity type classes.</span></span>

``` powershell
Scaffold-DbContext ... -ContextDir Data -OutputDir Models
```

``` Console
dotnet ef dbcontext scaffold ... --context-dir Data --output-dir Models
```

## <a name="how-it-works"></a><span data-ttu-id="8121c-145">Cómo funciona</span><span class="sxs-lookup"><span data-stu-id="8121c-145">How it works</span></span>

<span data-ttu-id="8121c-146">Ingeniería inversa comienza por leer el esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="8121c-146">Reverse engineering starts by reading the database schema.</span></span> <span data-ttu-id="8121c-147">Lee información sobre tablas, columnas, restricciones e índices.</span><span class="sxs-lookup"><span data-stu-id="8121c-147">It reads information about tables, columns, constraints, and indexes.</span></span>

<span data-ttu-id="8121c-148">A continuación, usa la información de esquema para crear un modelo de EF Core.</span><span class="sxs-lookup"><span data-stu-id="8121c-148">Next, it uses the schema information to create an EF Core model.</span></span> <span data-ttu-id="8121c-149">Las tablas se utilizan para crear tipos de entidad; las columnas se utilizan para crear las propiedades; y las claves externas se utilizan para crear las relaciones.</span><span class="sxs-lookup"><span data-stu-id="8121c-149">Tables are used to create entity types; columns are used to create properties; and foreign keys are used to create relationships.</span></span>

<span data-ttu-id="8121c-150">Por último, el modelo se usa para generar código.</span><span class="sxs-lookup"><span data-stu-id="8121c-150">Finally, the model is used to generate code.</span></span> <span data-ttu-id="8121c-151">Se ha aplicado scaffolding a la correspondiente entidad clases, la API Fluent y datos de las anotaciones de tipo con el fin de volver a crear el mismo modelo de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8121c-151">The corresponding entity type classes, Fluent API, and data annotations are scaffolded in order to re-create the same model from your app.</span></span>

## <a name="what-doesnt-work"></a><span data-ttu-id="8121c-152">¿Qué no funciona</span><span class="sxs-lookup"><span data-stu-id="8121c-152">What doesn't work</span></span>

<span data-ttu-id="8121c-153">No todo sobre un modelo puede representarse mediante un esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="8121c-153">Not everything about a model can be represented using a database schema.</span></span> <span data-ttu-id="8121c-154">Por ejemplo, información sobre **jerarquías de herencia**, **tipos de propiedad**, y **dividir la tabla** no están presentes en el esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="8121c-154">For example, information about **inheritance hierarchies**, **owned types**, and **table splitting** are not present in the database schema.</span></span> <span data-ttu-id="8121c-155">Por este motivo, estas construcciones no se nunca se ingeniería inversa.</span><span class="sxs-lookup"><span data-stu-id="8121c-155">Because of this, these constructs will never be reverse engineered.</span></span>

<span data-ttu-id="8121c-156">Además, **algunos tipos de columna** es posible que no se admite el proveedor de EF Core.</span><span class="sxs-lookup"><span data-stu-id="8121c-156">In addition, **some column types** may not be supported by the EF Core provider.</span></span> <span data-ttu-id="8121c-157">Estas columnas no se incluirán en el modelo.</span><span class="sxs-lookup"><span data-stu-id="8121c-157">These columns won't be included in the model.</span></span>

<span data-ttu-id="8121c-158">EF Core requiere tener una clave de cada tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="8121c-158">EF Core requires every entity type to have a key.</span></span> <span data-ttu-id="8121c-159">Las tablas, sin embargo, no se deben especificar una clave principal.</span><span class="sxs-lookup"><span data-stu-id="8121c-159">Tables, however, aren't required to specify a primary key.</span></span> <span data-ttu-id="8121c-160">**Las tablas sin una clave principal** son actualmente no se realizó ingeniería inversa.</span><span class="sxs-lookup"><span data-stu-id="8121c-160">**Tables without a primary key** are currently not reverse engineered.</span></span>

<span data-ttu-id="8121c-161">Puede definir **tokens de simultaneidad** en un modelo de EF Core para evitar que dos usuarios actualizando la misma entidad al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="8121c-161">You can define **concurrency tokens** in an EF Core model to prevent two users from updating the same entity at the same time.</span></span> <span data-ttu-id="8121c-162">Algunas bases de datos tienen un tipo especial para representar este tipo de columna (por ejemplo, rowversion en SQL Server), en cuyo caso de que la podemos invertir diseñar esta información; Sin embargo, otros tokens de simultaneidad no se invertirá ingeniería.</span><span class="sxs-lookup"><span data-stu-id="8121c-162">Some databases have a special type to represent this type of column (for example, rowversion in SQL Server) in which case we can reverse engineer this information; however, other concurrency tokens will not be reverse engineered.</span></span>

## <a name="customizing-the-model"></a><span data-ttu-id="8121c-163">Personalizar el modelo</span><span class="sxs-lookup"><span data-stu-id="8121c-163">Customizing the model</span></span>

<span data-ttu-id="8121c-164">El código generado por EF Core es el código.</span><span class="sxs-lookup"><span data-stu-id="8121c-164">The code generated by EF Core is your code.</span></span> <span data-ttu-id="8121c-165">No dude en cambiar.</span><span class="sxs-lookup"><span data-stu-id="8121c-165">Feel free to change it.</span></span> <span data-ttu-id="8121c-166">Solo se regenerará si aplicar ingeniería inversa del mismo modelo nuevo.</span><span class="sxs-lookup"><span data-stu-id="8121c-166">It will only be regenerated if you reverse engineer the same model again.</span></span> <span data-ttu-id="8121c-167">Representa el código con scaffolding *una* modelo que puede utilizarse para tener acceso a la base de datos, pero es ciertamente no la *sólo* modelo que se puede usar.</span><span class="sxs-lookup"><span data-stu-id="8121c-167">The scaffolded code represents *one* model that can be used to access the database, but it's certainly not the *only* model that can be used.</span></span>

<span data-ttu-id="8121c-168">Personalizar las clases de tipos de entidad y la clase DbContext para ajustarlo a sus necesidades.</span><span class="sxs-lookup"><span data-stu-id="8121c-168">Customize the entity type classes and DbContext class to fit your needs.</span></span> <span data-ttu-id="8121c-169">Por ejemplo, puede cambiar el nombre de tipos y propiedades, introducir a las jerarquías de herencia o dividir una tabla en a varias entidades.</span><span class="sxs-lookup"><span data-stu-id="8121c-169">For example, you may choose to rename types and properties, introduce inheritance hierarchies, or split a table into to multiple entities.</span></span> <span data-ttu-id="8121c-170">También puede quitar índices no únicos, sin usar secuencias y las propiedades de navegación, las propiedades escalares opcionales y los nombres de restricción del modelo.</span><span class="sxs-lookup"><span data-stu-id="8121c-170">You can also remove non-unique indexes, unused sequences and navigation properties, optional scalar properties, and constraint names from the model.</span></span>

<span data-ttu-id="8121c-171">También se pueden agregar constructores adicionales, métodos, propiedades, etcetera.</span><span class="sxs-lookup"><span data-stu-id="8121c-171">You can also add additional constructors, methods, properties, etc.</span></span> <span data-ttu-id="8121c-172">uso de otra clase parcial en un archivo independiente.</span><span class="sxs-lookup"><span data-stu-id="8121c-172">using another partial class in a separate file.</span></span> <span data-ttu-id="8121c-173">Este enfoque funciona incluso cuando se desea aplicar el modelo de ingeniería inversa a nuevo.</span><span class="sxs-lookup"><span data-stu-id="8121c-173">This approach works even when you intend to reverse engineer the model again.</span></span>

## <a name="updating-the-model"></a><span data-ttu-id="8121c-174">Actualización del modelo</span><span class="sxs-lookup"><span data-stu-id="8121c-174">Updating the model</span></span>

<span data-ttu-id="8121c-175">Después de realizar cambios en la base de datos, debe actualizar el modelo de EF Core para reflejar dichos cambios.</span><span class="sxs-lookup"><span data-stu-id="8121c-175">After making changes to the database, you may need to update your EF Core model to reflect those changes.</span></span> <span data-ttu-id="8121c-176">Si los cambios de la base de datos son simples, puede ser más fácil realizar manualmente los cambios en el modelo de EF Core.</span><span class="sxs-lookup"><span data-stu-id="8121c-176">If the database changes are simple, it may be easiest just to manually make the changes to your EF Core model.</span></span> <span data-ttu-id="8121c-177">Por ejemplo, cambiar el nombre de una tabla o columna, quitar una columna o la actualización de un tipo de columna es triviales cambios realizados en el código.</span><span class="sxs-lookup"><span data-stu-id="8121c-177">For example, renaming a table or column, removing a column, or updating a column's type are trivial changes to make in code.</span></span>

<span data-ttu-id="8121c-178">Los cambios más importantes, sin embargo, no son tan fácil hacer manualmente.</span><span class="sxs-lookup"><span data-stu-id="8121c-178">More significant changes, however, are not as easy make manually.</span></span> <span data-ttu-id="8121c-179">Un flujo de trabajo común es invertir diseñar el modelo de la base de datos nuevo con `-Force` (PMC) o `--force` (CLI) para sobrescribir el modelo existente con uno actualizado.</span><span class="sxs-lookup"><span data-stu-id="8121c-179">One common workflow is to reverse engineer the model from the database again using `-Force` (PMC) or `--force` (CLI) to overwrite the existing model with an updated one.</span></span>

<span data-ttu-id="8121c-180">Otra característica habitualmente solicitada es la capacidad para actualizar el modelo de la base de datos conservando la personalización, como cambios de nombre, las jerarquías de tipos, etcetera. Usar problema [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) para realizar un seguimiento del progreso de esta característica.</span><span class="sxs-lookup"><span data-stu-id="8121c-180">Another commonly requested feature is the ability to update the model from the database while preserving customization like renames, type hierarchies, etc. Use issue [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) to track the progress of this feature.</span></span>

> [!WARNING]
> <span data-ttu-id="8121c-181">Si la ingeniería inversa el modelo de la base de datos nuevo, se perderán los cambios realizados en los archivos.</span><span class="sxs-lookup"><span data-stu-id="8121c-181">If you reverse engineer the model from the database again, any changes you've made to the files will be lost.</span></span>
