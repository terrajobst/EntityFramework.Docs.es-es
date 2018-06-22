---
title: Herencia (base de datos relacional -) EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
ms.technology: entity-framework-core
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 22eed0002b5903d3cfd18a7e4af0fcd2d46a5c4c
ms.sourcegitcommit: d2434edbfa6fbcee7287e33b4915033b796e417e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2018
ms.locfileid: "29152371"
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

## <a name="configuring-the-discriminator-property"></a>Configurar la propiedad discriminador

En los ejemplos anteriores, se crea el discriminador como un [sombrear a la propiedad](xref:core/modeling/shadow-properties) en la entidad base de la jerarquía. Puesto que es una propiedad en el modelo, se puede configurar igual que otras propiedades. Por ejemplo, para establecer la longitud máxima, cuando se usa el valor predeterminado, discriminador por convención:

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

El discriminador también puede asignarse a una propiedad CLR real en la entidad. Por ejemplo:
```C#
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("BlogType");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public string BlogType { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

Combinar estos dos cosas es posible tanto del discriminador se asignan a una propiedad real y configurarlo:
```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
