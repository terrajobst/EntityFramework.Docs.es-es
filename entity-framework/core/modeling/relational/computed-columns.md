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
# <a name="computed-columns"></a>Columnas calculadas

> [!NOTE]  
> La configuración de esta sección se aplica a bases de datos relacionales en general. Los métodos de extensión que se muestran a continuación estarán disponibles cuando instale un proveedor de base de datos relacional (debido al paquete compartido *Microsoft.EntityFrameworkCore.Relational*).

Una columna calculada es una columna cuyo valor se calcula en la base de datos. Una columna calculada puede usar otras columnas en la tabla para calcular su valor.

## <a name="conventions"></a>Convenciones

Por convención, las columnas calculadas no se crean en el modelo.

## <a name="data-annotations"></a>Anotaciones de datos

No se pueden configurar las columnas calculadas con las anotaciones de datos.

## <a name="fluent-api"></a>API fluida

Puede usar la API Fluent para especificar que una propiedad se debe asignar a una columna calculada.

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
