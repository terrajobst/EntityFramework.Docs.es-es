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
# <a name="shadow-properties"></a>Propiedades de sombra

Las propiedades de sombra son propiedades que no están definidas en la clase de entidad de .NET pero que se definen para ese tipo de entidad en el modelo de EF Core. El valor y el estado de estas propiedades se mantienen únicamente en el seguimiento de cambios.

Las propiedades Shadow son útiles cuando hay datos en la base de datos que no se deben exponer en los tipos de entidad asignados. Se usan con mayor frecuencia para las propiedades de clave externa, donde la relación entre dos entidades se representa mediante un valor de clave externa en la base de datos, pero la relación se administra en los tipos de entidad mediante las propiedades de navegación entre los tipos de entidad.

Los valores de las propiedades Shadow se pueden obtener y `ChangeTracker` cambiar a través de la API.

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

Se puede hacer referencia a las propiedades Shadow en consultas LINQ `EF.Property` a través del método estático.

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a>Convenciones

Las propiedades de sombra se pueden crear por Convención cuando se detecta una relación, pero no se encuentra ninguna propiedad de clave externa en la clase de entidad dependiente. En este caso, se introducirá una propiedad de clave externa de sombra. La propiedad Shadow Foreign Key se denominará `<navigation property name><principal key property name>` (la navegación en la entidad dependiente, que apunta a la entidad principal, se utiliza para la nomenclatura). Si el nombre de la propiedad de clave principal incluye el nombre de la propiedad de navegación, el nombre `<principal key property name>`será simplemente. Si no hay ninguna propiedad de navegación en la entidad dependiente, el nombre del tipo de entidad de seguridad se usa en su lugar.

Por ejemplo, la siguiente lista de código dará como resultado `BlogId` la inclusión de una propiedad Shadow `Post` en la entidad.

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

## <a name="data-annotations"></a>Anotaciones de datos

No se pueden crear propiedades de sombra con anotaciones de datos.

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para configurar las propiedades de las instantáneas. Una vez que haya llamado a la sobrecarga `Property` de la cadena de, puede encadenar cualquiera de las llamadas de configuración que desee para otras propiedades.

Si el nombre proporcionado al `Property` método coincide con el nombre de una propiedad existente (una propiedad Shadow o una definida en la clase de entidad), el código configurará esa propiedad existente en lugar de introducir una nueva propiedad Shadow.

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
