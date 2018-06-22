---
title: Asignación de tabla - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
ms.technology: entity-framework-core
uid: core/modeling/relational/tables
ms.openlocfilehash: 73957d9c77e6856bfb65e10e6b373c337101f7d9
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052735"
---
# <a name="table-mapping"></a><span data-ttu-id="40c22-102">Asignación de tabla</span><span class="sxs-lookup"><span data-stu-id="40c22-102">Table Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="40c22-103">La configuración de esta sección es aplicable a bases de datos relacionales en general.</span><span class="sxs-lookup"><span data-stu-id="40c22-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="40c22-104">Los métodos de extensión que se muestra a continuación pasará a estar disponibles cuando se instala un proveedor de base de datos relacional (porque el recurso compartido *Microsoft.EntityFrameworkCore.Relational* paquete).</span><span class="sxs-lookup"><span data-stu-id="40c22-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="40c22-105">Asignación de tabla identifica qué datos de la tabla deben consultar desde y se guardan en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="40c22-105">Table mapping identifies which table data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="40c22-106">Convenciones</span><span class="sxs-lookup"><span data-stu-id="40c22-106">Conventions</span></span>

<span data-ttu-id="40c22-107">Por convención, cada entidad se configurará para asignar a una tabla con el mismo nombre que el `DbSet<TEntity>` que expone la entidad en el contexto derivada.</span><span class="sxs-lookup"><span data-stu-id="40c22-107">By convention, each entity will be setup to map to a table with the same name as the `DbSet<TEntity>` property that exposes the entity on the derived context.</span></span> <span data-ttu-id="40c22-108">Si no hay ningún `DbSet<TEntity>` se incluye para la entidad determinada, se usa el nombre de clase.</span><span class="sxs-lookup"><span data-stu-id="40c22-108">If no `DbSet<TEntity>` is included for the given entity, the class name is used.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="40c22-109">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="40c22-109">Data Annotations</span></span>

<span data-ttu-id="40c22-110">Puede usar anotaciones de datos para configurar la tabla que se asigna a un tipo.</span><span class="sxs-lookup"><span data-stu-id="40c22-110">You can use Data Annotations to configure the table that a type maps to.</span></span>

``` csharp
using System.ComponentModel.DataAnnotations.Schema;
```
``` csharp
[Table("blogs")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="40c22-111">También puede especificar un esquema al que pertenece la tabla.</span><span class="sxs-lookup"><span data-stu-id="40c22-111">You can also specify a schema that the table belongs to.</span></span>

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="40c22-112">API fluida</span><span class="sxs-lookup"><span data-stu-id="40c22-112">Fluent API</span></span>

<span data-ttu-id="40c22-113">Puede usar la API fluida para configurar la tabla que se asigna a un tipo.</span><span class="sxs-lookup"><span data-stu-id="40c22-113">You can use the Fluent API to configure the table that a type maps to.</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
```
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .ToTable("blogs");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="40c22-114">También puede especificar un esquema al que pertenece la tabla.</span><span class="sxs-lookup"><span data-stu-id="40c22-114">You can also specify a schema that the table belongs to.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/TableAndSchema.cs?highlight=2)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .ToTable("blogs", schema: "blogging");
```
