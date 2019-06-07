---
title: Reemplazar propiedades - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: 4029539f3642f539a427f5901577d4df96c00f30
ms.sourcegitcommit: 119058fefd7f35952048f783ada68be9aa612256
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749704"
---
# <a name="shadow-properties"></a>Propiedades reemplazadas

Reemplazar propiedades son propiedades que no estén definidas en la clase de entidad. NET, pero se han definido para ese tipo de entidad en el modelo de EF Core. El valor y el estado de estas propiedades se mantiene únicamente en el seguimiento de cambios.

Reemplazar propiedades son útiles cuando hay datos en la base de datos que no se debe exponer en los tipos de entidad asignada. Más a menudo se utilizan para las propiedades de clave externa, donde la relación entre dos entidades se representa mediante un valor de clave externa en la base de datos, pero la relación se administra en los tipos de entidad mediante propiedades de navegación entre los tipos de entidad.

Los valores de propiedad de sombra, pueden obtener y cambiar a través de la `ChangeTracker` API.

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

Se pueden hacer referencia a las propiedades reemplazadas en las consultas LINQ a través de la `EF.Property` método estático.

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a>Convenciones

Propiedades de instantáneas se pueden crear por convención cuando se detecta una relación, pero ninguna propiedad de clave externa se encuentra en la clase de entidad dependiente. En este caso, se introducirá una propiedad de clave externa de la sombra. La propiedad de clave externa de la sombra se denominará `<navigation property name><principal key property name>` (el panel de navegación de la entidad dependiente, que apunta a la entidad de seguridad, se usa para la nomenclatura). Si el nombre de propiedad de clave de entidad de seguridad incluye el nombre de la propiedad de navegación, entonces será el nombre `<principal key property name>`. Si no hay ninguna propiedad de navegación en la entidad dependiente, se usa el nombre del tipo de entidad de seguridad en su lugar.

Por ejemplo, la lista de código siguiente producirá un `BlogId` propiedad sombra que se presentan a la `Post` entidad.

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

No se pueden crear las propiedades reemplazadas con las anotaciones de datos.

## <a name="fluent-api"></a>API fluida

Puede usar la API Fluent para configurar las propiedades de la sombra. Después de haber llamado a la sobrecarga de la cadena de `Property` puede encadenar cualquiera de las llamadas de configuración que lo haría para otras propiedades.

Si el nombre proporcionado a la `Property` método coincide con el nombre de una propiedad existente (una propiedad reemplazada o uno definido en la clase de entidad), a continuación, configurará el código de esa propiedad existente en lugar de introducir una nueva propiedad reemplazada.

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
