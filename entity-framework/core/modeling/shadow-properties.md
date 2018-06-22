---
title: Reemplazar propiedades - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
ms.technology: entity-framework-core
uid: core/modeling/shadow-properties
ms.openlocfilehash: 8c7f70789ddc6ebd24f9bfae931069834db593c2
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2017
ms.locfileid: "26053555"
---
# <a name="shadow-properties"></a><span data-ttu-id="074e7-102">Reemplazar propiedades</span><span class="sxs-lookup"><span data-stu-id="074e7-102">Shadow Properties</span></span>

<span data-ttu-id="074e7-103">Reemplazar propiedades son propiedades que no estén definidos en la clase de entidad de .NET pero están definidos para ese tipo de entidad en el modelo de EF Core.</span><span class="sxs-lookup"><span data-stu-id="074e7-103">Shadow properties are properties that are not defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="074e7-104">El valor y el estado de estas propiedades se mantiene únicamente en la herramienta de seguimiento de cambios.</span><span class="sxs-lookup"><span data-stu-id="074e7-104">The value and state of these properties is maintained purely in the Change Tracker.</span></span>

<span data-ttu-id="074e7-105">Reemplazar propiedades son útiles cuando hay datos en la base de datos que no debe mostrarse en los tipos de entidad asignada.</span><span class="sxs-lookup"><span data-stu-id="074e7-105">Shadow properties are useful when there is data in the database that should not be exposed on the mapped entity types.</span></span> <span data-ttu-id="074e7-106">Se utiliza frecuentemente para propiedades de clave externa, donde la relación entre dos entidades se representa mediante un valor de clave externa en la base de datos, pero la relación está administrada en los tipos de entidad mediante propiedades de navegación entre los tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="074e7-106">They are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span>

<span data-ttu-id="074e7-107">Los valores de propiedad de instantáneas, pueden obtener y cambiar a través de la `ChangeTracker` API.</span><span class="sxs-lookup"><span data-stu-id="074e7-107">Shadow property values can be obtained and changed through the `ChangeTracker` API.</span></span>

``` csharp
   context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="074e7-108">Pueden hacer referencia a propiedades de instantáneas en las consultas LINQ a través de la `EF.Property` método estático.</span><span class="sxs-lookup"><span data-stu-id="074e7-108">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method.</span></span>

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a><span data-ttu-id="074e7-109">Convenciones</span><span class="sxs-lookup"><span data-stu-id="074e7-109">Conventions</span></span>

<span data-ttu-id="074e7-110">Propiedades de instantáneas se pueden crear por convención cuando se detecta una relación, pero no se encuentra ninguna propiedad de clave externa en la clase de entidad dependiente.</span><span class="sxs-lookup"><span data-stu-id="074e7-110">Shadow properties can be created by convention when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span> <span data-ttu-id="074e7-111">En este caso, se introducirán una propiedad de clave externa de la sombra.</span><span class="sxs-lookup"><span data-stu-id="074e7-111">In this case, a shadow foreign key property will be introduced.</span></span> <span data-ttu-id="074e7-112">La propiedad de clave externa de instantáneas se denominará `<navigation property name><principal key property name>` (el panel de navegación en la entidad dependiente, que señala a la entidad de seguridad, se utiliza para la nomenclatura).</span><span class="sxs-lookup"><span data-stu-id="074e7-112">The shadow foreign key property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="074e7-113">Si el nombre de propiedad de clave de entidad de seguridad incluye el nombre de la propiedad de navegación, el nombre solo será `<principal key property name>`.</span><span class="sxs-lookup"><span data-stu-id="074e7-113">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="074e7-114">Si no hay ninguna propiedad de navegación en la entidad dependiente, el nombre de tipo de entidad se utiliza en su lugar.</span><span class="sxs-lookup"><span data-stu-id="074e7-114">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="074e7-115">Por ejemplo, la lista de código siguiente producirá un `BlogId` propiedad de instantáneas que se introducen en la `Post` entidad.</span><span class="sxs-lookup"><span data-stu-id="074e7-115">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/ShadowForeignKey.cs)] -->
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

## <a name="data-annotations"></a><span data-ttu-id="074e7-116">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="074e7-116">Data Annotations</span></span>

<span data-ttu-id="074e7-117">Reemplazar propiedades no pueden crearse con las anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="074e7-117">Shadow properties can not be created with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="074e7-118">API fluida</span><span class="sxs-lookup"><span data-stu-id="074e7-118">Fluent API</span></span>

<span data-ttu-id="074e7-119">Puede usar la API fluida para configurar las propiedades de la sombra.</span><span class="sxs-lookup"><span data-stu-id="074e7-119">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="074e7-120">Una vez que se llamó a la sobrecarga de la cadena de `Property` se pueden encadenar cualquiera de las llamadas de configuración que lo haría para otras propiedades.</span><span class="sxs-lookup"><span data-stu-id="074e7-120">Once you have called the string overload of `Property` you can chain any of the configuration calls you would for other properties.</span></span>

<span data-ttu-id="074e7-121">Si el nombre proporcionado a la `Property` método coincide con el nombre de una propiedad existente (una propiedad de instantáneas o uno definido en la clase de entidad), a continuación, configurará el código de esa propiedad existente en lugar de introducir una nueva propiedad de instantáneas.</span><span class="sxs-lookup"><span data-stu-id="074e7-121">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/ShadowProperty.cs?highlight=7,8)] -->
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
