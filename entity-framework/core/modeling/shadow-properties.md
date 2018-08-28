---
title: Reemplazar propiedades - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: b7b7b10642564dfa3dbc05755188b5b5c63e0d03
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993807"
---
# <a name="shadow-properties"></a><span data-ttu-id="98fcf-102">Propiedades reemplazadas</span><span class="sxs-lookup"><span data-stu-id="98fcf-102">Shadow Properties</span></span>

<span data-ttu-id="98fcf-103">Reemplazar propiedades son propiedades que no estén definidas en la clase de entidad. NET, pero se han definido para ese tipo de entidad en el modelo de EF Core.</span><span class="sxs-lookup"><span data-stu-id="98fcf-103">Shadow properties are properties that are not defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="98fcf-104">El valor y el estado de estas propiedades se mantiene únicamente en el seguimiento de cambios.</span><span class="sxs-lookup"><span data-stu-id="98fcf-104">The value and state of these properties is maintained purely in the Change Tracker.</span></span>

<span data-ttu-id="98fcf-105">Reemplazar propiedades son útiles cuando hay datos en la base de datos que no se debe exponer en los tipos de entidad asignada.</span><span class="sxs-lookup"><span data-stu-id="98fcf-105">Shadow properties are useful when there is data in the database that should not be exposed on the mapped entity types.</span></span> <span data-ttu-id="98fcf-106">Más a menudo se utilizan para las propiedades de clave externa, donde la relación entre dos entidades se representa mediante un valor de clave externa en la base de datos, pero la relación se administra en los tipos de entidad mediante propiedades de navegación entre los tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="98fcf-106">They are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span>

<span data-ttu-id="98fcf-107">Los valores de propiedad de sombra, pueden obtener y cambiar a través de la `ChangeTracker` API.</span><span class="sxs-lookup"><span data-stu-id="98fcf-107">Shadow property values can be obtained and changed through the `ChangeTracker` API.</span></span>

``` csharp
   context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="98fcf-108">Se pueden hacer referencia a las propiedades reemplazadas en las consultas LINQ a través de la `EF.Property` método estático.</span><span class="sxs-lookup"><span data-stu-id="98fcf-108">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method.</span></span>

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a><span data-ttu-id="98fcf-109">Convenciones</span><span class="sxs-lookup"><span data-stu-id="98fcf-109">Conventions</span></span>

<span data-ttu-id="98fcf-110">Propiedades de instantáneas se pueden crear por convención cuando se detecta una relación, pero ninguna propiedad de clave externa se encuentra en la clase de entidad dependiente.</span><span class="sxs-lookup"><span data-stu-id="98fcf-110">Shadow properties can be created by convention when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span> <span data-ttu-id="98fcf-111">En este caso, se introducirá una propiedad de clave externa de la sombra.</span><span class="sxs-lookup"><span data-stu-id="98fcf-111">In this case, a shadow foreign key property will be introduced.</span></span> <span data-ttu-id="98fcf-112">La propiedad de clave externa de la sombra se denominará `<navigation property name><principal key property name>` (el panel de navegación de la entidad dependiente, que apunta a la entidad de seguridad, se usa para la nomenclatura).</span><span class="sxs-lookup"><span data-stu-id="98fcf-112">The shadow foreign key property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="98fcf-113">Si el nombre de propiedad de clave de entidad de seguridad incluye el nombre de la propiedad de navegación, entonces será el nombre `<principal key property name>`.</span><span class="sxs-lookup"><span data-stu-id="98fcf-113">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="98fcf-114">Si no hay ninguna propiedad de navegación en la entidad dependiente, se usa el nombre del tipo de entidad de seguridad en su lugar.</span><span class="sxs-lookup"><span data-stu-id="98fcf-114">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="98fcf-115">Por ejemplo, la lista de código siguiente producirá un `BlogId` propiedad sombra que se presentan a la `Post` entidad.</span><span class="sxs-lookup"><span data-stu-id="98fcf-115">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="98fcf-116">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="98fcf-116">Data Annotations</span></span>

<span data-ttu-id="98fcf-117">No se pueden crear las propiedades reemplazadas con las anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="98fcf-117">Shadow properties can not be created with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="98fcf-118">API fluida</span><span class="sxs-lookup"><span data-stu-id="98fcf-118">Fluent API</span></span>

<span data-ttu-id="98fcf-119">Puede usar la API Fluent para configurar las propiedades de la sombra.</span><span class="sxs-lookup"><span data-stu-id="98fcf-119">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="98fcf-120">Después de haber llamado a la sobrecarga de la cadena de `Property` puede encadenar cualquiera de las llamadas de configuración que lo haría para otras propiedades.</span><span class="sxs-lookup"><span data-stu-id="98fcf-120">Once you have called the string overload of `Property` you can chain any of the configuration calls you would for other properties.</span></span>

<span data-ttu-id="98fcf-121">Si el nombre proporcionado a la `Property` método coincide con el nombre de una propiedad existente (una propiedad reemplazada o uno definido en la clase de entidad), a continuación, configurará el código de esa propiedad existente en lugar de introducir una nueva propiedad reemplazada.</span><span class="sxs-lookup"><span data-stu-id="98fcf-121">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

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
