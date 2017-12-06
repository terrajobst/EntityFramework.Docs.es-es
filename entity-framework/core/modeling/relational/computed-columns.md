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
# <a name="computed-columns"></a>Columnas calculadas

> [!NOTE]  
> La configuración de esta sección es aplicable a bases de datos relacionales en general. Los métodos de extensión que se muestra a continuación pasará a estar disponibles cuando se instala un proveedor de base de datos relacional (porque el recurso compartido *Microsoft.EntityFrameworkCore.Relational* paquete).

Una columna calculada es una columna cuyo valor se calcula en la base de datos. Una columna calculada puede usar otras columnas de la tabla para calcular su valor.

## <a name="conventions"></a>Convenciones

Por convención, las columnas calculadas no se crean en el modelo.

## <a name="data-annotations"></a>Anotaciones de datos

Las columnas calculadas no pueden configurarse con las anotaciones de datos.

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para especificar que una propiedad se debe asignar a una columna calculada.

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
