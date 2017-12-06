---
title: "Longitud máxima - Core EF"
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
---
# <a name="maximum-length"></a><span data-ttu-id="68d98-102">Longitud máxima</span><span class="sxs-lookup"><span data-stu-id="68d98-102">Maximum Length</span></span>

<span data-ttu-id="68d98-103">Configuración de una longitud máxima, proporciona una sugerencia al almacén de datos sobre el tipo de datos adecuado que se usará para una propiedad determinada.</span><span class="sxs-lookup"><span data-stu-id="68d98-103">Configuring a maximum length provides a hint to the data store about the appropriate data type to use for a given property.</span></span> <span data-ttu-id="68d98-104">Longitud máxima solo se aplica a tipos de datos de matriz, como `string` y `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="68d98-104">Maximum length only applies to array data types, such as `string` and `byte[]`.</span></span>

> [!NOTE]  
> <span data-ttu-id="68d98-105">Entity Framework no realiza ninguna validación de longitud máxima antes de pasar datos al proveedor.</span><span class="sxs-lookup"><span data-stu-id="68d98-105">Entity Framework does not do any validation of maximum length before passing data to the provider.</span></span> <span data-ttu-id="68d98-106">Es el almacén de datos o proveedor para validar si es necesario.</span><span class="sxs-lookup"><span data-stu-id="68d98-106">It is up to the provider or data store to validate if appropriate.</span></span> <span data-ttu-id="68d98-107">Por ejemplo, cuando se dirige a SQL Server, si se supera la longitud máxima se producirá una excepción como el tipo de datos de la columna subyacente no permitirá un exceso de datos que se almacenará.</span><span class="sxs-lookup"><span data-stu-id="68d98-107">For example, when targeting SQL Server, exceeding the maximum length will result in an exception as the data type of the underlying column will not allow excess data to be stored.</span></span>

## <a name="conventions"></a><span data-ttu-id="68d98-108">Convenciones</span><span class="sxs-lookup"><span data-stu-id="68d98-108">Conventions</span></span>

<span data-ttu-id="68d98-109">Por convención, se mantiene hasta el proveedor de base de datos para elegir un tipo de datos adecuado para las propiedades.</span><span class="sxs-lookup"><span data-stu-id="68d98-109">By convention, it is left up to the database provider to choose an appropriate data type for properties.</span></span> <span data-ttu-id="68d98-110">Para las propiedades que tienen una longitud, el proveedor de base de datos suele elegir un tipo de datos que permite la mayor longitud de datos.</span><span class="sxs-lookup"><span data-stu-id="68d98-110">For properties that have a length, the database provider will generally choose a data type that allows for the longest length of data.</span></span> <span data-ttu-id="68d98-111">Por ejemplo, Microsoft SQL Server usará `nvarchar(max)` para `string` propiedades (o `nvarchar(450)` si la columna se utiliza como una clave).</span><span class="sxs-lookup"><span data-stu-id="68d98-111">For example, Microsoft SQL Server will use `nvarchar(max)` for `string` properties (or `nvarchar(450)` if the column is used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="68d98-112">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="68d98-112">Data Annotations</span></span>

<span data-ttu-id="68d98-113">Puede usar las anotaciones de datos para configurar una longitud máxima para una propiedad.</span><span class="sxs-lookup"><span data-stu-id="68d98-113">You can use the Data Annotations to configure a maximum length for a property.</span></span> <span data-ttu-id="68d98-114">En este ejemplo, como el destino de SQL Server, esto produciría el `nvarchar(500)` tipo de datos que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="68d98-114">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/MaxLength.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [MaxLength(500)]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="68d98-115">API fluida</span><span class="sxs-lookup"><span data-stu-id="68d98-115">Fluent API</span></span>

<span data-ttu-id="68d98-116">Puede usar la API fluida para configurar una longitud máxima para una propiedad.</span><span class="sxs-lookup"><span data-stu-id="68d98-116">You can use the Fluent API to configure a maximum length for a property.</span></span> <span data-ttu-id="68d98-117">En este ejemplo, como el destino de SQL Server, esto produciría el `nvarchar(500)` tipo de datos que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="68d98-117">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

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
