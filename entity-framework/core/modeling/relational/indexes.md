---
title: "Índices - Core EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
ms.technology: entity-framework-core
uid: core/modeling/relational/indexes
ms.openlocfilehash: 683b580bb155e0416f13c5d63e3280078fbcee21
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
---
# <a name="indexes"></a><span data-ttu-id="f72db-102">Índices</span><span class="sxs-lookup"><span data-stu-id="f72db-102">Indexes</span></span>

> [!NOTE]  
> <span data-ttu-id="f72db-103">La configuración de esta sección es aplicable a bases de datos relacionales en general.</span><span class="sxs-lookup"><span data-stu-id="f72db-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="f72db-104">Los métodos de extensión que se muestra a continuación pasará a estar disponibles cuando se instala un proveedor de base de datos relacional (porque el recurso compartido *Microsoft.EntityFrameworkCore.Relational* paquete).</span><span class="sxs-lookup"><span data-stu-id="f72db-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="f72db-105">Un índice en una base de datos relacional se asigna para el mismo concepto que un índice en el núcleo de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f72db-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="f72db-106">Convenciones</span><span class="sxs-lookup"><span data-stu-id="f72db-106">Conventions</span></span>

<span data-ttu-id="f72db-107">Por convención, se denominan índices `IX_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="f72db-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="f72db-108">En los índices compuestos `<property name>` se convierte en una lista de subrayado separado de los nombres de propiedad.</span><span class="sxs-lookup"><span data-stu-id="f72db-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="f72db-109">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="f72db-109">Data Annotations</span></span>

<span data-ttu-id="f72db-110">Los índices no pueden configurarse mediante las anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="f72db-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="f72db-111">API fluida</span><span class="sxs-lookup"><span data-stu-id="f72db-111">Fluent API</span></span>

<span data-ttu-id="f72db-112">Puede usar la API fluida para configurar el nombre de un índice.</span><span class="sxs-lookup"><span data-stu-id="f72db-112">You can use the Fluent API to configure the name of an index.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/IndexName.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .HasName("Index_Url");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
