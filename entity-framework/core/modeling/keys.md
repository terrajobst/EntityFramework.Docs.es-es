---
title: Claves (principal) - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
ms.technology: entity-framework-core
uid: core/modeling/keys
ms.openlocfilehash: f3bf3c7f2a28e065b350fe000a5164406cd5ca08
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
---
# <a name="keys-primary"></a><span data-ttu-id="6d115-102">Claves (principal)</span><span class="sxs-lookup"><span data-stu-id="6d115-102">Keys (primary)</span></span>

<span data-ttu-id="6d115-103">Una clave actúa como el principal identificador único para cada instancia de entidad.</span><span class="sxs-lookup"><span data-stu-id="6d115-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="6d115-104">Cuando se usa una base de datos relacional se asigna al concepto de un *clave principal*.</span><span class="sxs-lookup"><span data-stu-id="6d115-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="6d115-105">También puede configurar un identificador único que no es la clave principal (vea [claves alternativas](alternate-keys.md) para obtener más información).</span><span class="sxs-lookup"><span data-stu-id="6d115-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

## <a name="conventions"></a><span data-ttu-id="6d115-106">Convenciones</span><span class="sxs-lookup"><span data-stu-id="6d115-106">Conventions</span></span>

<span data-ttu-id="6d115-107">Por convención, una propiedad denominada `Id` o `<type name>Id` se configurará como la clave de una entidad.</span><span class="sxs-lookup"><span data-stu-id="6d115-107">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="6d115-108">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="6d115-108">Data Annotations</span></span>

<span data-ttu-id="6d115-109">Puede usar anotaciones de datos para configurar una sola propiedad para que sea la clave de una entidad.</span><span class="sxs-lookup"><span data-stu-id="6d115-109">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

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

## <a name="fluent-api"></a><span data-ttu-id="6d115-110">API fluida</span><span class="sxs-lookup"><span data-stu-id="6d115-110">Fluent API</span></span>

<span data-ttu-id="6d115-111">Puede usar la API fluida para configurar una sola propiedad para que sea la clave de una entidad.</span><span class="sxs-lookup"><span data-stu-id="6d115-111">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

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

<span data-ttu-id="6d115-112">También puede usar la API fluida para configurar varias propiedades para que sea la clave de una entidad (conocida como una clave compuesta).</span><span class="sxs-lookup"><span data-stu-id="6d115-112">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="6d115-113">Las claves compuestas solamente puede configurarse mediante la API fluida: convenciones no configurará nunca una clave compuesta y no puede usar anotaciones de datos para configurar uno.</span><span class="sxs-lookup"><span data-stu-id="6d115-113">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

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
