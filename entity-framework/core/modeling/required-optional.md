---
title: Propiedades obligatorias y opcionales - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
ms.technology: entity-framework-core
uid: core/modeling/required-optional
ms.openlocfilehash: 2af1d49e12ef980f81cb9c00556dee471673ccae
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
---
# <a name="required-and-optional-properties"></a>Propiedades necesarias y opcionales

Una propiedad se considera opcional si es válido para que pueda contener `null`. Si `null` no es un valor válido que se asignará a una propiedad, a continuación, se considera que una propiedad necesaria.

## <a name="conventions"></a>Convenciones

Por convención, una propiedad cuyo tipo CLR puede contener valores nulos se configurarán como opcionales (`string`, `int?`, `byte[]`, etcetera.). Propiedades cuyo tipo CLR no puede contener valores nulos se configurará según sea necesario (`int`, `decimal`, `bool`, etcetera.).

> [!NOTE]  
> Una propiedad cuyo tipo CLR no puede contener null no se puede configurar como opcionales. La propiedad siempre se considerarán requeridos por Entity Framework.

## <a name="data-annotations"></a>Anotaciones de datos

Puede usar anotaciones de datos para indicar que una propiedad es obligatoria.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [Required]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para indicar que una propiedad es obligatoria.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Url)
            .IsRequired();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
