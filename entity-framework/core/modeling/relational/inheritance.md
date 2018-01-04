---
title: Herencia (base de datos relacional -) EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
ms.technology: entity-framework-core
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 55286adf08a6a1c3286b7059d747a62e1feffd22
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/22/2017
---
# <a name="inheritance-relational-database"></a>Herencia (base de datos relacional)

> [!NOTE]  
> La configuración de esta sección se aplica a bases de datos relacionales en general. Los métodos de extensión que se muestran a continuación estarán disponibles cuando instale un proveedor de base de datos relacional (debido al paquete compartido *Microsoft.EntityFrameworkCore.Relational*).

Herencia en el modelo EF se utiliza para controlar cómo se representa la herencia en las clases de entidad en la base de datos.

> [!NOTE]  
> Actualmente, solo el patrón de tabla por jerarquía (TPH) se implementa en EF Core. Otros patrones comunes como tabla por tipo (TPT) y tabla-por-hormigón-type (TCP) no están disponibles.

## <a name="conventions"></a>Convenciones

Por convención, la herencia se asignarán utilizando el modelo de tabla por jerarquía (TPH). TPH usa una sola tabla para almacenar los datos para todos los tipos en la jerarquía. Se usa una columna discriminadora para identificar qué tipo de cada fila representa.

EF Core solo configurará herencia si dos o más tipos heredados están incluidos explícitamente en el modelo (vea [herencia](../inheritance.md) para obtener más detalles).

A continuación se muestra un ejemplo que muestra un escenario de herencia simple y los datos almacenados en una tabla de base de datos relacional mediante el patrón TPH. El *discriminador* columna identifica el tipo de *Blog* se almacena en cada fila.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/Conventions/Samples/InheritanceDbSets.cs)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<RssBlog> RssBlogs { get; set; }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

![imagen](_static/inheritance-tph-data.png)

## <a name="data-annotations"></a>Anotaciones de datos

No puede usar anotaciones de datos para configurar la herencia.

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para configurar el nombre y tipo de la columna discriminadora y los valores que se usan para identificar cada tipo en la jerarquía.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/InheritanceTPHDiscriminator.cs?highlight=7,8,9,10)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("blog_type")
            .HasValue<Blog>("blog_base")
            .HasValue<RssBlog>("blog_rss");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```
