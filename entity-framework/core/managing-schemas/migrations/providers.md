---
title: Migraciones con varios proveedores - Core EF
author: bricelam
ms.author: bricelam
ms.date: 11/8/2017
ms.technology: entity-framework-core
ms.openlocfilehash: d950e74ed4cef7d4274aabcf3eda7b0b735574c6
ms.sourcegitcommit: 2ef0a4a90b01edd22b9206f8729b8de459ef8cab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2018
---
<a name="migrations-with-multiple-providers"></a><span data-ttu-id="0f80c-102">Migraciones con varios proveedores</span><span class="sxs-lookup"><span data-stu-id="0f80c-102">Migrations with Multiple Providers</span></span>
==================================
<span data-ttu-id="0f80c-103">El [herramientas básicas de EF] [ 1] solo aplique la técnica scaffolding las migraciones para el proveedor activo.</span><span class="sxs-lookup"><span data-stu-id="0f80c-103">The [EF Core Tools][1] only scaffold migrations for the active provider.</span></span> <span data-ttu-id="0f80c-104">A veces, sin embargo, puede que desee utilizar más de un proveedor (por ejemplo Microsoft SQL Server y SQLite) con su DbContext.</span><span class="sxs-lookup"><span data-stu-id="0f80c-104">Sometimes, however, you may want to use more than one provider (for example Microsoft SQL Server and SQLite) with your DbContext.</span></span> <span data-ttu-id="0f80c-105">Hay dos formas de controlar esto con las migraciones.</span><span class="sxs-lookup"><span data-stu-id="0f80c-105">There are two ways to handle this with Migrations.</span></span> <span data-ttu-id="0f80c-106">Puede mantener dos conjuntos de migraciones: uno para cada proveedor--o combinación de ellas en un solo conjunto que puede trabajar en ambos.</span><span class="sxs-lookup"><span data-stu-id="0f80c-106">You can maintain two sets of migrations--one for each provider--or merge them into a single set that can work on both.</span></span>

<a name="two-migration-sets"></a><span data-ttu-id="0f80c-107">Dos conjuntos de migración</span><span class="sxs-lookup"><span data-stu-id="0f80c-107">Two migration sets</span></span>
------------------
<span data-ttu-id="0f80c-108">En el primer enfoque y generar dos migraciones para cada cambio de modelo.</span><span class="sxs-lookup"><span data-stu-id="0f80c-108">In the first approach, you generate two migrations for each model change.</span></span>

<span data-ttu-id="0f80c-109">Una manera de hacer esto es colocar cada conjunto de migración [en un ensamblado independiente] [ 2] y cambiar manualmente el proveedor activo (y el ensamblado de migraciones) entre dos migraciones de agregar.</span><span class="sxs-lookup"><span data-stu-id="0f80c-109">One way to do this is to put each migration set [in a separate assembly][2] and manually switch the active provider (and migrations assembly) between adding the two migrations.</span></span>

<span data-ttu-id="0f80c-110">Otro enfoque que facilita el trabajo con las herramientas consiste en crear un nuevo tipo que deriva de su DbContext e invalida el proveedor activo.</span><span class="sxs-lookup"><span data-stu-id="0f80c-110">Another approach that makes working with the tools easier is to create a new type that derives from your DbContext and overrides the active provider.</span></span> <span data-ttu-id="0f80c-111">Este tipo se utiliza en el diseño de tiempo cuando se agregan o aplicar las migraciones.</span><span class="sxs-lookup"><span data-stu-id="0f80c-111">This type is used at design time when adding or applying migrations.</span></span>

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> <span data-ttu-id="0f80c-112">Puesto que cada conjunto de migración utiliza sus propios tipos de DbContext, este enfoque no requiere el uso de un ensamblado de migraciones independientes.</span><span class="sxs-lookup"><span data-stu-id="0f80c-112">Since each migration set uses its own DbContext types, this approach doesn't require using a separate migrations assembly.</span></span>

<span data-ttu-id="0f80c-113">Al agregar nueva migración, especificar los tipos de contexto.</span><span class="sxs-lookup"><span data-stu-id="0f80c-113">When adding new migration, specify the context types.</span></span>

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```
``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

> [!TIP]
> <span data-ttu-id="0f80c-114">No es necesario especificar el directorio de salida para las migraciones posteriores desde que se crearon como nodos relacionados hasta el último.</span><span class="sxs-lookup"><span data-stu-id="0f80c-114">You don't need to specify the output directory for subsequent migrations since they are created as siblings to the last one.</span></span>

<a name="one-migration-set"></a><span data-ttu-id="0f80c-115">Conjunto de una migración</span><span class="sxs-lookup"><span data-stu-id="0f80c-115">One migration set</span></span>
-----------------
<span data-ttu-id="0f80c-116">Si no le gusta tener dos conjuntos de migraciones, puede combinarlas manualmente en un único conjunto que se pueden aplicar a ambos proveedores.</span><span class="sxs-lookup"><span data-stu-id="0f80c-116">If you don't like having two sets of migrations, you can manually combine them into a single set that can be applied to both providers.</span></span>

<span data-ttu-id="0f80c-117">Las anotaciones pueden coexistir desde un proveedor omite cualquier anotación que no comprende.</span><span class="sxs-lookup"><span data-stu-id="0f80c-117">Annotations can coexist since a provider ignores any annotations that it doesn't understand.</span></span> <span data-ttu-id="0f80c-118">Por ejemplo, una columna de clave principal que funciona con Microsoft SQL Server y SQLite podría ser similar al siguiente.</span><span class="sxs-lookup"><span data-stu-id="0f80c-118">For example, a primary key column that works with both Microsoft SQL Server and SQLite might look like this.</span></span>

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

<span data-ttu-id="0f80c-119">Si solo se pueden aplicar operaciones en un proveedor (o sean más diferente entre proveedores), utilice el `ActiveProvider` propiedad para indicar qué proveedor está activo.</span><span class="sxs-lookup"><span data-stu-id="0f80c-119">If operations can only be applied on one provider (or they're differently between providers), use the `ActiveProvider` property to tell which provider is active.</span></span>

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
