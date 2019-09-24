---
title: 'Valores predeterminados: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
uid: core/modeling/relational/default-values
ms.openlocfilehash: 0d3613606f21a78e22cfe0ee752ea982a6a17f93
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196984"
---
# <a name="default-values"></a><span data-ttu-id="6477c-102">Valores predeterminados</span><span class="sxs-lookup"><span data-stu-id="6477c-102">Default Values</span></span>

> [!NOTE]  
> <span data-ttu-id="6477c-103">La configuración de esta sección se aplica a bases de datos relacionales en general.</span><span class="sxs-lookup"><span data-stu-id="6477c-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="6477c-104">Los métodos de extensión que se muestran a continuación estarán disponibles cuando instale un proveedor de base de datos relacional (debido al paquete compartido *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="6477c-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="6477c-105">El valor predeterminado de una columna es el valor que se insertará si se inserta una nueva fila pero no se especifica ningún valor para la columna.</span><span class="sxs-lookup"><span data-stu-id="6477c-105">The default value of a column is the value that will be inserted if a new row is inserted but no value is specified for the column.</span></span>

## <a name="conventions"></a><span data-ttu-id="6477c-106">Convenciones</span><span class="sxs-lookup"><span data-stu-id="6477c-106">Conventions</span></span>

<span data-ttu-id="6477c-107">Por Convención, no se configura un valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="6477c-107">By convention, a default value is not configured.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="6477c-108">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="6477c-108">Data Annotations</span></span>

<span data-ttu-id="6477c-109">No se puede establecer un valor predeterminado mediante anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="6477c-109">You can not set a default value using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="6477c-110">API fluida</span><span class="sxs-lookup"><span data-stu-id="6477c-110">Fluent API</span></span>

<span data-ttu-id="6477c-111">Puede usar la API fluida para especificar el valor predeterminado de una propiedad.</span><span class="sxs-lookup"><span data-stu-id="6477c-111">You can use the Fluent API to specify the default value for a property.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/DefaultValue.cs?highlight=9)] -->
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

<span data-ttu-id="6477c-112">También puede especificar un fragmento de SQL que se utiliza para calcular el valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="6477c-112">You can also specify a SQL fragment that is used to calculate the default value.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/DefaultValueSql.cs?highlight=9)] -->
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
