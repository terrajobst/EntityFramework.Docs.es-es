---
title: Propiedades obligatorias y opcionales - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: b6716a5b03e1afc2933e317d606ef50f986c22c7
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995502"
---
# <a name="required-and-optional-properties"></a>Propiedades obligatorias y opcionales

Una propiedad se considera opcional si es válido para poder contener `null`. Si `null` no es un valor válido que se asignará a una propiedad, a continuación, se considera una propiedad necesaria.

## <a name="conventions"></a>Convenciones

Por convención, una propiedad cuyo tipo CLR puede contener valores nulos se configurarán como opcional (`string`, `int?`, `byte[]`, etcetera.). Las propiedades cuyo tipo CLR no puede contener valores nulos se configurará según sea necesario (`int`, `decimal`, `bool`, etcetera.).

> [!NOTE]  
> Una propiedad cuyo tipo CLR no puede contener null no se puede configurar como opcionales. La propiedad siempre se considerará requerido por Entity Framework.

## <a name="data-annotations"></a>Anotaciones de datos

Puede usar anotaciones de datos para indicar que la propiedad es obligatoria.

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

Puede usar la API Fluent para indicar que la propiedad es obligatoria.

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
