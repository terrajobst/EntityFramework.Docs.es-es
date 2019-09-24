---
title: 'Propiedades de sombra: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: 5fdc4c50c295f73d0fa5eef3518adf4d3eb95599
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197701"
---
# <a name="shadow-properties"></a><span data-ttu-id="d9040-102">Propiedades de sombra</span><span class="sxs-lookup"><span data-stu-id="d9040-102">Shadow Properties</span></span>

<span data-ttu-id="d9040-103">Las propiedades de sombra son propiedades que no están definidas en la clase de entidad de .NET pero que se definen para ese tipo de entidad en el modelo de EF Core.</span><span class="sxs-lookup"><span data-stu-id="d9040-103">Shadow properties are properties that are not defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="d9040-104">El valor y el estado de estas propiedades se mantienen únicamente en el seguimiento de cambios.</span><span class="sxs-lookup"><span data-stu-id="d9040-104">The value and state of these properties is maintained purely in the Change Tracker.</span></span>

<span data-ttu-id="d9040-105">Las propiedades Shadow son útiles cuando hay datos en la base de datos que no se deben exponer en los tipos de entidad asignados.</span><span class="sxs-lookup"><span data-stu-id="d9040-105">Shadow properties are useful when there is data in the database that should not be exposed on the mapped entity types.</span></span> <span data-ttu-id="d9040-106">Se usan con mayor frecuencia para las propiedades de clave externa, donde la relación entre dos entidades se representa mediante un valor de clave externa en la base de datos, pero la relación se administra en los tipos de entidad mediante las propiedades de navegación entre los tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="d9040-106">They are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span>

<span data-ttu-id="d9040-107">Los valores de las propiedades Shadow se pueden obtener y `ChangeTracker` cambiar a través de la API.</span><span class="sxs-lookup"><span data-stu-id="d9040-107">Shadow property values can be obtained and changed through the `ChangeTracker` API.</span></span>

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="d9040-108">Se puede hacer referencia a las propiedades Shadow en consultas LINQ `EF.Property` a través del método estático.</span><span class="sxs-lookup"><span data-stu-id="d9040-108">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method.</span></span>

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a><span data-ttu-id="d9040-109">Convenciones</span><span class="sxs-lookup"><span data-stu-id="d9040-109">Conventions</span></span>

<span data-ttu-id="d9040-110">Las propiedades de sombra se pueden crear por Convención cuando se detecta una relación, pero no se encuentra ninguna propiedad de clave externa en la clase de entidad dependiente.</span><span class="sxs-lookup"><span data-stu-id="d9040-110">Shadow properties can be created by convention when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span> <span data-ttu-id="d9040-111">En este caso, se introducirá una propiedad de clave externa de sombra.</span><span class="sxs-lookup"><span data-stu-id="d9040-111">In this case, a shadow foreign key property will be introduced.</span></span> <span data-ttu-id="d9040-112">La propiedad Shadow Foreign Key se denominará `<navigation property name><principal key property name>` (la navegación en la entidad dependiente, que apunta a la entidad principal, se utiliza para la nomenclatura).</span><span class="sxs-lookup"><span data-stu-id="d9040-112">The shadow foreign key property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="d9040-113">Si el nombre de la propiedad de clave principal incluye el nombre de la propiedad de navegación, el nombre `<principal key property name>`será simplemente.</span><span class="sxs-lookup"><span data-stu-id="d9040-113">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="d9040-114">Si no hay ninguna propiedad de navegación en la entidad dependiente, el nombre del tipo de entidad de seguridad se usa en su lugar.</span><span class="sxs-lookup"><span data-stu-id="d9040-114">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="d9040-115">Por ejemplo, la siguiente lista de código dará como resultado `BlogId` la inclusión de una propiedad Shadow `Post` en la entidad.</span><span class="sxs-lookup"><span data-stu-id="d9040-115">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/ShadowForeignKey.cs)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="d9040-116">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="d9040-116">Data Annotations</span></span>

<span data-ttu-id="d9040-117">No se pueden crear propiedades de sombra con anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="d9040-117">Shadow properties can not be created with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="d9040-118">API fluida</span><span class="sxs-lookup"><span data-stu-id="d9040-118">Fluent API</span></span>

<span data-ttu-id="d9040-119">Puede usar la API fluida para configurar las propiedades de las instantáneas.</span><span class="sxs-lookup"><span data-stu-id="d9040-119">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="d9040-120">Una vez que haya llamado a la sobrecarga `Property` de la cadena de, puede encadenar cualquiera de las llamadas de configuración que desee para otras propiedades.</span><span class="sxs-lookup"><span data-stu-id="d9040-120">Once you have called the string overload of `Property` you can chain any of the configuration calls you would for other properties.</span></span>

<span data-ttu-id="d9040-121">Si el nombre proporcionado al `Property` método coincide con el nombre de una propiedad existente (una propiedad Shadow o una definida en la clase de entidad), el código configurará esa propiedad existente en lugar de introducir una nueva propiedad Shadow.</span><span class="sxs-lookup"><span data-stu-id="d9040-121">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/ShadowProperty.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property<DateTime>("LastUpdated");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
