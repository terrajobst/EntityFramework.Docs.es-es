---
title: Columnas calculadas - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
ms.technology: entity-framework-core
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: 95312504286bd34cc666b5a21273835c4b35d379
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
---
# <a name="computed-columns"></a><span data-ttu-id="4f8c8-102">Columnas calculadas</span><span class="sxs-lookup"><span data-stu-id="4f8c8-102">Computed Columns</span></span>

> [!NOTE]  
> <span data-ttu-id="4f8c8-103">La configuración de esta sección es aplicable a bases de datos relacionales en general.</span><span class="sxs-lookup"><span data-stu-id="4f8c8-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="4f8c8-104">Los métodos de extensión que se muestra a continuación pasará a estar disponibles cuando se instala un proveedor de base de datos relacional (porque el recurso compartido *Microsoft.EntityFrameworkCore.Relational* paquete).</span><span class="sxs-lookup"><span data-stu-id="4f8c8-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="4f8c8-105">Una columna calculada es una columna cuyo valor se calcula en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4f8c8-105">A computed column is a column whose value is calculated in the database.</span></span> <span data-ttu-id="4f8c8-106">Una columna calculada puede usar otras columnas de la tabla para calcular su valor.</span><span class="sxs-lookup"><span data-stu-id="4f8c8-106">A computed column can use other columns in the table to calculate its value.</span></span>

## <a name="conventions"></a><span data-ttu-id="4f8c8-107">Convenciones</span><span class="sxs-lookup"><span data-stu-id="4f8c8-107">Conventions</span></span>

<span data-ttu-id="4f8c8-108">Por convención, las columnas calculadas no se crean en el modelo.</span><span class="sxs-lookup"><span data-stu-id="4f8c8-108">By convention, computed columns are not created in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="4f8c8-109">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="4f8c8-109">Data Annotations</span></span>

<span data-ttu-id="4f8c8-110">Las columnas calculadas no pueden configurarse con las anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="4f8c8-110">Computed columns can not be configured with Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="4f8c8-111">API fluida</span><span class="sxs-lookup"><span data-stu-id="4f8c8-111">Fluent API</span></span>

<span data-ttu-id="4f8c8-112">Puede usar la API fluida para especificar que una propiedad se debe asignar a una columna calculada.</span><span class="sxs-lookup"><span data-stu-id="4f8c8-112">You can use the Fluent API to specify that a property should map to a computed column.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/ComputedColumn.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Person> People { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Person>()
            .Property(p => p.DisplayName)
            .HasComputedColumnSql("[LastName] + ', ' + [FirstName]");
    }
}

public class Person
{
    public int PersonId { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string DisplayName { get; set; }
}
```
