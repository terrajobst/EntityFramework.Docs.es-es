---
title: Migraciones con varios proveedores - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/8/2017
ms.openlocfilehash: 7ae695037992323337a780cda29d8c8ed8a13458
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997978"
---
<a name="migrations-with-multiple-providers"></a><span data-ttu-id="2e91d-102">Migraciones con varios proveedores</span><span class="sxs-lookup"><span data-stu-id="2e91d-102">Migrations with Multiple Providers</span></span>
==================================
<span data-ttu-id="2e91d-103">El [herramientas de EF Core] [ 1] solo aplicar la técnica scaffolding migraciones para el proveedor activo.</span><span class="sxs-lookup"><span data-stu-id="2e91d-103">The [EF Core Tools][1] only scaffold migrations for the active provider.</span></span> <span data-ttu-id="2e91d-104">A veces, sin embargo, es posible que desee usar más de un proveedor (por ejemplo Microsoft SQL Server y SQLite) con DbContext.</span><span class="sxs-lookup"><span data-stu-id="2e91d-104">Sometimes, however, you may want to use more than one provider (for example Microsoft SQL Server and SQLite) with your DbContext.</span></span> <span data-ttu-id="2e91d-105">Hay dos formas de controlar esto con las migraciones.</span><span class="sxs-lookup"><span data-stu-id="2e91d-105">There are two ways to handle this with Migrations.</span></span> <span data-ttu-id="2e91d-106">Puede mantener dos conjuntos de migraciones: uno para cada proveedor--o merge en un único conjunto de que pueda trabajar en ambos.</span><span class="sxs-lookup"><span data-stu-id="2e91d-106">You can maintain two sets of migrations--one for each provider--or merge them into a single set that can work on both.</span></span>

<a name="two-migration-sets"></a><span data-ttu-id="2e91d-107">Dos conjuntos de migración</span><span class="sxs-lookup"><span data-stu-id="2e91d-107">Two migration sets</span></span>
------------------
<span data-ttu-id="2e91d-108">En el primer enfoque, generar dos migraciones para cada cambio del modelo.</span><span class="sxs-lookup"><span data-stu-id="2e91d-108">In the first approach, you generate two migrations for each model change.</span></span>

<span data-ttu-id="2e91d-109">Una forma de hacerlo es colocar cada conjunto de migración [en un ensamblado independiente] [ 2] y cambie manualmente el proveedor activo (y el ensamblado de migraciones) entre las dos migraciones de agregar.</span><span class="sxs-lookup"><span data-stu-id="2e91d-109">One way to do this is to put each migration set [in a separate assembly][2] and manually switch the active provider (and migrations assembly) between adding the two migrations.</span></span>

<span data-ttu-id="2e91d-110">Otro enfoque que facilita el trabajo con las herramientas es crear un nuevo tipo que deriva de DbContext y reemplaza el proveedor activo.</span><span class="sxs-lookup"><span data-stu-id="2e91d-110">Another approach that makes working with the tools easier is to create a new type that derives from your DbContext and overrides the active provider.</span></span> <span data-ttu-id="2e91d-111">Este tipo se usa en el diseño de tiempo cuando se agregan o aplicar las migraciones.</span><span class="sxs-lookup"><span data-stu-id="2e91d-111">This type is used at design time when adding or applying migrations.</span></span>

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> <span data-ttu-id="2e91d-112">Puesto que cada conjunto de migración utiliza sus propios tipos de DbContext, este enfoque no requiere el uso de un ensamblado independiente de las migraciones.</span><span class="sxs-lookup"><span data-stu-id="2e91d-112">Since each migration set uses its own DbContext types, this approach doesn't require using a separate migrations assembly.</span></span>

<span data-ttu-id="2e91d-113">Al agregar la nueva migración, especificar los tipos de contexto.</span><span class="sxs-lookup"><span data-stu-id="2e91d-113">When adding new migration, specify the context types.</span></span>

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```
``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

> [!TIP]
> <span data-ttu-id="2e91d-114">No es necesario especificar el directorio de salida para las migraciones posteriores desde que se crearon como elementos relacionados a la última de ellas.</span><span class="sxs-lookup"><span data-stu-id="2e91d-114">You don't need to specify the output directory for subsequent migrations since they are created as siblings to the last one.</span></span>

<a name="one-migration-set"></a><span data-ttu-id="2e91d-115">Migración de un conjunto</span><span class="sxs-lookup"><span data-stu-id="2e91d-115">One migration set</span></span>
-----------------
<span data-ttu-id="2e91d-116">Si no le gusta tener dos conjuntos de migraciones, puede combinarlos manualmente en un único conjunto que se puede aplicar a ambos proveedores.</span><span class="sxs-lookup"><span data-stu-id="2e91d-116">If you don't like having two sets of migrations, you can manually combine them into a single set that can be applied to both providers.</span></span>

<span data-ttu-id="2e91d-117">Las anotaciones pueden coexistir puesto que un proveedor omite cualquier anotación que no comprenda.</span><span class="sxs-lookup"><span data-stu-id="2e91d-117">Annotations can coexist since a provider ignores any annotations that it doesn't understand.</span></span> <span data-ttu-id="2e91d-118">Por ejemplo, una columna de clave principal que funciona con Microsoft SQL Server y SQLite podría ser similar al siguiente.</span><span class="sxs-lookup"><span data-stu-id="2e91d-118">For example, a primary key column that works with both Microsoft SQL Server and SQLite might look like this.</span></span>

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

<span data-ttu-id="2e91d-119">Si solo se pueden aplicar operaciones en un proveedor (o son una forma diferente entre los proveedores), use el `ActiveProvider` propiedad para indicar a qué proveedor está activo.</span><span class="sxs-lookup"><span data-stu-id="2e91d-119">If operations can only be applied on one provider (or they're differently between providers), use the `ActiveProvider` property to tell which provider is active.</span></span>

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
