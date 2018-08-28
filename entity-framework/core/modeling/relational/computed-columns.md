---
title: Columnas calculadas - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: b88efdf69e5100e4eff55f3a41925d2d8e7c3178
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993958"
---
# <a name="computed-columns"></a><span data-ttu-id="45a94-102">Columnas calculadas</span><span class="sxs-lookup"><span data-stu-id="45a94-102">Computed Columns</span></span>

> [!NOTE]  
> <span data-ttu-id="45a94-103">La configuración de esta sección se aplica a bases de datos relacionales en general.</span><span class="sxs-lookup"><span data-stu-id="45a94-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="45a94-104">Los métodos de extensión que se muestran a continuación estarán disponibles cuando instale un proveedor de base de datos relacional (debido al paquete compartido *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="45a94-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="45a94-105">Una columna calculada es una columna cuyo valor se calcula en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="45a94-105">A computed column is a column whose value is calculated in the database.</span></span> <span data-ttu-id="45a94-106">Una columna calculada puede usar otras columnas en la tabla para calcular su valor.</span><span class="sxs-lookup"><span data-stu-id="45a94-106">A computed column can use other columns in the table to calculate its value.</span></span>

## <a name="conventions"></a><span data-ttu-id="45a94-107">Convenciones</span><span class="sxs-lookup"><span data-stu-id="45a94-107">Conventions</span></span>

<span data-ttu-id="45a94-108">Por convención, las columnas calculadas no se crean en el modelo.</span><span class="sxs-lookup"><span data-stu-id="45a94-108">By convention, computed columns are not created in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="45a94-109">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="45a94-109">Data Annotations</span></span>

<span data-ttu-id="45a94-110">No se pueden configurar las columnas calculadas con las anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="45a94-110">Computed columns can not be configured with Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="45a94-111">API fluida</span><span class="sxs-lookup"><span data-stu-id="45a94-111">Fluent API</span></span>

<span data-ttu-id="45a94-112">Puede usar la API Fluent para especificar que una propiedad se debe asignar a una columna calculada.</span><span class="sxs-lookup"><span data-stu-id="45a94-112">You can use the Fluent API to specify that a property should map to a computed column.</span></span>

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
