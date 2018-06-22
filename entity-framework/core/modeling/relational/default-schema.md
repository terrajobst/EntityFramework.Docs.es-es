---
title: Esquema predeterminado - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
ms.technology: entity-framework-core
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 26106deb2d4e35ecf33e97790a83f9af77991aed
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052755"
---
# <a name="default-schema"></a><span data-ttu-id="a4cf4-102">Esquema predeterminado</span><span class="sxs-lookup"><span data-stu-id="a4cf4-102">Default Schema</span></span>

> [!NOTE]  
> <span data-ttu-id="a4cf4-103">La configuración de esta sección es aplicable a bases de datos relacionales en general.</span><span class="sxs-lookup"><span data-stu-id="a4cf4-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="a4cf4-104">Los métodos de extensión que se muestra a continuación pasará a estar disponibles cuando se instala un proveedor de base de datos relacional (porque el recurso compartido *Microsoft.EntityFrameworkCore.Relational* paquete).</span><span class="sxs-lookup"><span data-stu-id="a4cf4-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="a4cf4-105">El esquema predeterminado es el esquema de base de datos que si un esquema no está configurado explícitamente para ese objeto, se creará en objetos.</span><span class="sxs-lookup"><span data-stu-id="a4cf4-105">The default schema is the database schema that objects will be created in if a schema is not explicitly configured for that object.</span></span>

## <a name="conventions"></a><span data-ttu-id="a4cf4-106">Convenciones</span><span class="sxs-lookup"><span data-stu-id="a4cf4-106">Conventions</span></span>

<span data-ttu-id="a4cf4-107">Por convención, el proveedor de base de datos elegirá el esquema predeterminado más adecuado.</span><span class="sxs-lookup"><span data-stu-id="a4cf4-107">By convention, the database provider will choose the most appropriate default schema.</span></span> <span data-ttu-id="a4cf4-108">Por ejemplo, Microsoft SQL Server usará la `dbo` esquema y SQLite no utilizarán un esquema (ya que no se admiten esquemas en SQLite).</span><span class="sxs-lookup"><span data-stu-id="a4cf4-108">For example, Microsoft SQL Server will use the `dbo` schema and SQLite will not use a schema (since schemas are not supported in SQLite).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="a4cf4-109">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="a4cf4-109">Data Annotations</span></span>

<span data-ttu-id="a4cf4-110">No puede establecer el esquema predeterminado mediante las anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="a4cf4-110">You can not set the default schema using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="a4cf4-111">API fluida</span><span class="sxs-lookup"><span data-stu-id="a4cf4-111">Fluent API</span></span>

<span data-ttu-id="a4cf4-112">Puede usar la API fluida para especificar un esquema predeterminado.</span><span class="sxs-lookup"><span data-stu-id="a4cf4-112">You can use the Fluent API to specify a default schema.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultSchema.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasDefaultSchema("blogging");
    }
}
```
