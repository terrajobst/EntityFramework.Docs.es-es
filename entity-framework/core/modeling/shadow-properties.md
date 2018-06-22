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
# <a name="shadow-properties"></a>Reemplazar propiedades

Reemplazar propiedades son propiedades que no estén definidos en la clase de entidad de .NET pero están definidos para ese tipo de entidad en el modelo de EF Core. El valor y el estado de estas propiedades se mantiene únicamente en la herramienta de seguimiento de cambios.

Reemplazar propiedades son útiles cuando hay datos en la base de datos que no debe mostrarse en los tipos de entidad asignada. Se utiliza frecuentemente para propiedades de clave externa, donde la relación entre dos entidades se representa mediante un valor de clave externa en la base de datos, pero la relación está administrada en los tipos de entidad mediante propiedades de navegación entre los tipos de entidad.

Los valores de propiedad de instantáneas, pueden obtener y cambiar a través de la `ChangeTracker` API.

``` csharp
   context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

Pueden hacer referencia a propiedades de instantáneas en las consultas LINQ a través de la `EF.Property` método estático.

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a>Convenciones

Propiedades de instantáneas se pueden crear por convención cuando se detecta una relación, pero no se encuentra ninguna propiedad de clave externa en la clase de entidad dependiente. En este caso, se introducirán una propiedad de clave externa de la sombra. La propiedad de clave externa de instantáneas se denominará `<navigation property name><principal key property name>` (el panel de navegación en la entidad dependiente, que señala a la entidad de seguridad, se utiliza para la nomenclatura). Si el nombre de propiedad de clave de entidad de seguridad incluye el nombre de la propiedad de navegación, el nombre solo será `<principal key property name>`. Si no hay ninguna propiedad de navegación en la entidad dependiente, el nombre de tipo de entidad se utiliza en su lugar.

Por ejemplo, la lista de código siguiente producirá un `BlogId` propiedad de instantáneas que se introducen en la `Post` entidad.

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

## <a name="data-annotations"></a>Anotaciones de datos

Reemplazar propiedades no pueden crearse con las anotaciones de datos.

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para configurar las propiedades de la sombra. Una vez que se llamó a la sobrecarga de la cadena de `Property` se pueden encadenar cualquiera de las llamadas de configuración que lo haría para otras propiedades.

Si el nombre proporcionado a la `Property` método coincide con el nombre de una propiedad existente (una propiedad de instantáneas o uno definido en la clase de entidad), a continuación, configurará el código de esa propiedad existente en lugar de introducir una nueva propiedad de instantáneas.

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
