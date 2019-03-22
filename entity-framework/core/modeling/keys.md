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
# <a name="keys-primary"></a><span data-ttu-id="8d0ac-102">Claves (principal)</span><span class="sxs-lookup"><span data-stu-id="8d0ac-102">Keys (primary)</span></span>

<span data-ttu-id="8d0ac-103">Una clave actúa como identificador único para cada instancia de la entidad principal.</span><span class="sxs-lookup"><span data-stu-id="8d0ac-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="8d0ac-104">Cuando se usa una base de datos relacional se asigna al concepto de un *clave principal*.</span><span class="sxs-lookup"><span data-stu-id="8d0ac-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="8d0ac-105">También puede configurar un identificador único que no es la clave principal (consulte [claves alternativas](alternate-keys.md) para obtener más información).</span><span class="sxs-lookup"><span data-stu-id="8d0ac-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span> 

<span data-ttu-id="8d0ac-106">Uno de los métodos siguientes se puede usar el programa de instalación o crear una clave principal.</span><span class="sxs-lookup"><span data-stu-id="8d0ac-106">One of the following methods can be used to setup/create a primary key.</span></span>

## <a name="conventions"></a><span data-ttu-id="8d0ac-107">Convenciones</span><span class="sxs-lookup"><span data-stu-id="8d0ac-107">Conventions</span></span>

<span data-ttu-id="8d0ac-108">Por convención, una propiedad denominada `Id` o `<type name>Id` se configurarán como la clave de una entidad.</span><span class="sxs-lookup"><span data-stu-id="8d0ac-108">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="8d0ac-109">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="8d0ac-109">Data Annotations</span></span>

<span data-ttu-id="8d0ac-110">Puede usar anotaciones de datos para configurar una propiedad única para que sea la clave de una entidad.</span><span class="sxs-lookup"><span data-stu-id="8d0ac-110">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

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

## <a name="fluent-api"></a><span data-ttu-id="8d0ac-111">API fluida</span><span class="sxs-lookup"><span data-stu-id="8d0ac-111">Fluent API</span></span>

<span data-ttu-id="8d0ac-112">Puede usar la API Fluent para configurar una propiedad única para que sea la clave de una entidad.</span><span class="sxs-lookup"><span data-stu-id="8d0ac-112">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

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

<span data-ttu-id="8d0ac-113">También puede usar la API Fluent para configurar varias propiedades para que sea la clave de una entidad (conocida como una clave compuesta).</span><span class="sxs-lookup"><span data-stu-id="8d0ac-113">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="8d0ac-114">Las claves compuestas solo se pueden configurar mediante la API de Fluent: convenciones nunca realizará la instalación de una clave compuesta y no se pueden usar anotaciones de datos para configurar uno.</span><span class="sxs-lookup"><span data-stu-id="8d0ac-114">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

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
