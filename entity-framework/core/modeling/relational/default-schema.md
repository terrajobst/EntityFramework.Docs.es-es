---
title: Esquema predeterminado - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 800551bbadd0a9e8b5eb7070a8ccf6ed2407e3d2
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995371"
---
# <a name="default-schema"></a><span data-ttu-id="ede0b-102">Esquema predeterminado</span><span class="sxs-lookup"><span data-stu-id="ede0b-102">Default Schema</span></span>

> [!NOTE]  
> <span data-ttu-id="ede0b-103">La configuración de esta sección se aplica a bases de datos relacionales en general.</span><span class="sxs-lookup"><span data-stu-id="ede0b-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="ede0b-104">Los métodos de extensión que se muestran a continuación estarán disponibles cuando instale un proveedor de base de datos relacional (debido al paquete compartido *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="ede0b-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="ede0b-105">El esquema predeterminado es el esquema de base de datos que los objetos se crearán en si un esquema no está configurado explícitamente para ese objeto.</span><span class="sxs-lookup"><span data-stu-id="ede0b-105">The default schema is the database schema that objects will be created in if a schema is not explicitly configured for that object.</span></span>

## <a name="conventions"></a><span data-ttu-id="ede0b-106">Convenciones</span><span class="sxs-lookup"><span data-stu-id="ede0b-106">Conventions</span></span>

<span data-ttu-id="ede0b-107">Por convención, el proveedor de base de datos elegirá el esquema predeterminado más adecuado.</span><span class="sxs-lookup"><span data-stu-id="ede0b-107">By convention, the database provider will choose the most appropriate default schema.</span></span> <span data-ttu-id="ede0b-108">Por ejemplo, Microsoft SQL Server usará el `dbo` esquema y SQLite no usará un esquema (ya que no se admiten esquemas en SQLite).</span><span class="sxs-lookup"><span data-stu-id="ede0b-108">For example, Microsoft SQL Server will use the `dbo` schema and SQLite will not use a schema (since schemas are not supported in SQLite).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="ede0b-109">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="ede0b-109">Data Annotations</span></span>

<span data-ttu-id="ede0b-110">No se puede establecer el esquema predeterminado usando anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="ede0b-110">You can not set the default schema using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="ede0b-111">API fluida</span><span class="sxs-lookup"><span data-stu-id="ede0b-111">Fluent API</span></span>

<span data-ttu-id="ede0b-112">Puede usar la API Fluent para especificar un esquema predeterminado.</span><span class="sxs-lookup"><span data-stu-id="ede0b-112">You can use the Fluent API to specify a default schema.</span></span>

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
