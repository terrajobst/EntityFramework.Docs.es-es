---
title: Valores predeterminados - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
ms.technology: entity-framework-core
uid: core/modeling/relational/default-values
ms.openlocfilehash: 73b916b6d9f9c984c8ea010f2319eafa7d031a58
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
---
# <a name="default-values"></a><span data-ttu-id="b13d4-102">Valores predeterminados</span><span class="sxs-lookup"><span data-stu-id="b13d4-102">Default Values</span></span>

> [!NOTE]  
> <span data-ttu-id="b13d4-103">La configuración de esta sección es aplicable a bases de datos relacionales en general.</span><span class="sxs-lookup"><span data-stu-id="b13d4-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="b13d4-104">Los métodos de extensión que se muestra a continuación pasará a estar disponibles cuando se instala un proveedor de base de datos relacional (porque el recurso compartido *Microsoft.EntityFrameworkCore.Relational* paquete).</span><span class="sxs-lookup"><span data-stu-id="b13d4-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="b13d4-105">El valor predeterminado de una columna es el valor que se van a insertar si se inserta una fila nueva, pero se especifica ningún valor para la columna.</span><span class="sxs-lookup"><span data-stu-id="b13d4-105">The default value of a column is the value that will be inserted if a new row is inserted but no value is specified for the column.</span></span>

## <a name="conventions"></a><span data-ttu-id="b13d4-106">Convenciones</span><span class="sxs-lookup"><span data-stu-id="b13d4-106">Conventions</span></span>

<span data-ttu-id="b13d4-107">Por convención, un valor predeterminado no está configurado.</span><span class="sxs-lookup"><span data-stu-id="b13d4-107">By convention, a default value is not configured.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="b13d4-108">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="b13d4-108">Data Annotations</span></span>

<span data-ttu-id="b13d4-109">No puede establecer un valor predeterminado con las anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="b13d4-109">You can not set a default value using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="b13d4-110">API fluida</span><span class="sxs-lookup"><span data-stu-id="b13d4-110">Fluent API</span></span>

<span data-ttu-id="b13d4-111">Puede usar la API fluida para especificar el valor predeterminado para una propiedad.</span><span class="sxs-lookup"><span data-stu-id="b13d4-111">You can use the Fluent API to specify the default value for a property.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultValue.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Rating)
            .HasDefaultValue(3);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public int Rating { get; set; }
}
```

<span data-ttu-id="b13d4-112">También puede especificar un fragmento SQL que se usa para calcular el valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="b13d4-112">You can also specify a SQL fragment that is used to calculate the default value.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultValueSql.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Created)
            .HasDefaultValueSql("getdate()");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public DateTime Created { get; set; }
}
```
