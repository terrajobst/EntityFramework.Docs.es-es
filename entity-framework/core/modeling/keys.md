---
title: Claves (principal) - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 6272e323b83ccab2ed060a2ebbde1d1e8e353d66
ms.sourcegitcommit: eb8359b7ab3b0a1a08522faf67b703a00ecdcefd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "58319171"
---
# <a name="keys-primary"></a>Claves (principal)

Una clave actúa como identificador único para cada instancia de la entidad principal. Cuando se usa una base de datos relacional se asigna al concepto de un *clave principal*. También puede configurar un identificador único que no es la clave principal (consulte [claves alternativas](alternate-keys.md) para obtener más información). 

Uno de los métodos siguientes se puede usar el programa de instalación o crear una clave principal.

## <a name="conventions"></a>Convenciones

Por convención, una propiedad denominada `Id` o `<type name>Id` se configurarán como la clave de una entidad.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string Id { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyTypeNameId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string CarId { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="data-annotations"></a>Anotaciones de datos

Puede usar anotaciones de datos para configurar una propiedad única para que sea la clave de una entidad.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/KeySingle.cs?highlight=3,4)] -->
``` csharp
class Car
{
    [Key]
    public string LicensePlate { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="fluent-api"></a>API fluida

Puede usar la API Fluent para configurar una propiedad única para que sea la clave de una entidad.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/KeySingle.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasKey(c => c.LicensePlate);
    }
}

class Car
{
    public string LicensePlate { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

También puede usar la API Fluent para configurar varias propiedades para que sea la clave de una entidad (conocida como una clave compuesta). Las claves compuestas solo se pueden configurar mediante la API de Fluent: convenciones nunca realizará la instalación de una clave compuesta y no se pueden usar anotaciones de datos para configurar uno.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/KeyComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasKey(c => new { c.State, c.LicensePlate });
    }
}

class Car
{
    public string State { get; set; }
    public string LicensePlate { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```
