---
title: Claves principales - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
ms.technology: entity-framework-core
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: fcb1871149c0f20a2576864028b4171904de1982
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
---
# <a name="primary-keys"></a><span data-ttu-id="98503-102">Claves principales</span><span class="sxs-lookup"><span data-stu-id="98503-102">Primary Keys</span></span>

> [!NOTE]  
> <span data-ttu-id="98503-103">La configuración de esta sección es aplicable a bases de datos relacionales en general.</span><span class="sxs-lookup"><span data-stu-id="98503-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="98503-104">Los métodos de extensión que se muestra a continuación pasará a estar disponibles cuando se instala un proveedor de base de datos relacional (porque el recurso compartido *Microsoft.EntityFrameworkCore.Relational* paquete).</span><span class="sxs-lookup"><span data-stu-id="98503-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="98503-105">Se introdujo una restricción primary key para la clave de cada tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="98503-105">A primary key constraint is introduced for the key of each entity type.</span></span>

## <a name="conventions"></a><span data-ttu-id="98503-106">Convenciones</span><span class="sxs-lookup"><span data-stu-id="98503-106">Conventions</span></span>

<span data-ttu-id="98503-107">Por convención, la clave principal de la base de datos se denominará `PK_<type name>`.</span><span class="sxs-lookup"><span data-stu-id="98503-107">By convention, the primary key in the database will be named `PK_<type name>`.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="98503-108">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="98503-108">Data Annotations</span></span>

<span data-ttu-id="98503-109">No hay aspectos específicos de la base de datos relacional de una clave principal se pueden configurar mediante las anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="98503-109">No relational database specific aspects of a primary key can be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="98503-110">API fluida</span><span class="sxs-lookup"><span data-stu-id="98503-110">Fluent API</span></span>

<span data-ttu-id="98503-111">Puede utilizar la API fluida para configurar el nombre de la restricción primary key en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="98503-111">You can use the Fluent API to configure the name of the primary key constraint in the database.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/KeyName.cs?highlight=9)] -->
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
