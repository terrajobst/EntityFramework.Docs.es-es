---
title: 'Claves principales: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: bdb31964d717c64bddc28e7f1ce55b261e285a9b
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196954"
---
# <a name="primary-keys"></a><span data-ttu-id="c58d2-102">Claves principales</span><span class="sxs-lookup"><span data-stu-id="c58d2-102">Primary Keys</span></span>

> [!NOTE]  
> <span data-ttu-id="c58d2-103">La configuración de esta sección se aplica a bases de datos relacionales en general.</span><span class="sxs-lookup"><span data-stu-id="c58d2-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="c58d2-104">Los métodos de extensión que se muestran a continuación estarán disponibles cuando instale un proveedor de base de datos relacional (debido al paquete compartido *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="c58d2-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="c58d2-105">Se introduce una restricción primary key para la clave de cada tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="c58d2-105">A primary key constraint is introduced for the key of each entity type.</span></span>

## <a name="conventions"></a><span data-ttu-id="c58d2-106">Convenciones</span><span class="sxs-lookup"><span data-stu-id="c58d2-106">Conventions</span></span>

<span data-ttu-id="c58d2-107">Por Convención, la clave principal en la base de datos se `PK_<type name>`denominará.</span><span class="sxs-lookup"><span data-stu-id="c58d2-107">By convention, the primary key in the database will be named `PK_<type name>`.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="c58d2-108">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="c58d2-108">Data Annotations</span></span>

<span data-ttu-id="c58d2-109">No se puede configurar ningún aspecto específico de la base de datos relacional con anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="c58d2-109">No relational database specific aspects of a primary key can be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="c58d2-110">API fluida</span><span class="sxs-lookup"><span data-stu-id="c58d2-110">Fluent API</span></span>

<span data-ttu-id="c58d2-111">Puede usar la API fluida para configurar el nombre de la restricción primary key en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c58d2-111">You can use the Fluent API to configure the name of the primary key constraint in the database.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/KeyName.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasKey(b => b.BlogId)
            .HasName("PrimaryKey_BlogId");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
