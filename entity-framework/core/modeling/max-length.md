---
title: Longitud máxima - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
ms.technology: entity-framework-core
uid: core/modeling/max-length
ms.openlocfilehash: 7325c0c3328477473392bf9e7c82f1696bb4f424
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052675"
---
# <a name="maximum-length"></a>Longitud máxima

Configuración de una longitud máxima, proporciona una sugerencia al almacén de datos sobre el tipo de datos adecuado que se usará para una propiedad determinada. Longitud máxima solo se aplica a tipos de datos de matriz, como `string` y `byte[]`.

> [!NOTE]  
> Entity Framework no realiza ninguna validación de longitud máxima antes de pasar datos al proveedor. Es el almacén de datos o proveedor para validar si es necesario. Por ejemplo, cuando se dirige a SQL Server, si se supera la longitud máxima se producirá una excepción como el tipo de datos de la columna subyacente no permitirá un exceso de datos que se almacenará.

## <a name="conventions"></a>Convenciones

Por convención, se mantiene hasta el proveedor de base de datos para elegir un tipo de datos adecuado para las propiedades. Para las propiedades que tienen una longitud, el proveedor de base de datos suele elegir un tipo de datos que permite la mayor longitud de datos. Por ejemplo, Microsoft SQL Server usará `nvarchar(max)` para `string` propiedades (o `nvarchar(450)` si la columna se utiliza como una clave).

## <a name="data-annotations"></a>Anotaciones de datos

Puede usar las anotaciones de datos para configurar una longitud máxima para una propiedad. En este ejemplo, como el destino de SQL Server, esto produciría el `nvarchar(500)` tipo de datos que se va a usar.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/MaxLength.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [MaxLength(500)]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para configurar una longitud máxima para una propiedad. En este ejemplo, como el destino de SQL Server, esto produciría el `nvarchar(500)` tipo de datos que se va a usar.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/MaxLength.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Url)
            .HasMaxLength(500);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
