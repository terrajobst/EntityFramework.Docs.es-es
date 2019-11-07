---
title: Herencia (base de datos relacional)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 381d1878007bb78b359eb49649f4356f1e5eb04a
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655632"
---
# <a name="inheritance-relational-database"></a>Herencia (base de datos relacional)

> [!NOTE]  
> La configuración de esta sección se aplica a bases de datos relacionales en general. Los métodos de extensión que se muestran a continuación estarán disponibles cuando instale un proveedor de base de datos relacional (debido al paquete compartido *Microsoft.EntityFrameworkCore.Relational*).

La herencia en el modelo de EF se utiliza para controlar cómo se representa la herencia en las clases de entidad en la base de datos.

> [!NOTE]  
> Actualmente, solo el patrón de tabla por jerarquía (TPH) se implementa en EF Core. Otros patrones comunes como tabla por tipo (TPT) y tabla por tipo específico (TPC) no están disponibles todavía.

## <a name="conventions"></a>Convenciones

Por Convención, la herencia se asignará mediante el patrón de tabla por jerarquía (TPH). TPH usa una sola tabla para almacenar los datos de todos los tipos de la jerarquía. Una columna de discriminador se utiliza para identificar el tipo que representa cada fila.

EF Core solo configurará la herencia si dos o más tipos heredados se incluyen explícitamente en el modelo (consulte [herencia](../inheritance.md) para obtener más detalles).

A continuación se muestra un ejemplo en el que se muestra un escenario de herencia simple y los datos almacenados en una tabla de base de datos relacional mediante el patrón TPH. La columna *Discriminator* identifica el tipo de *blog* que se almacena en cada fila.

[!code-csharp[Main](../../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs#Model)]

![imagen](_static/inheritance-tph-data.png)

>[!NOTE]
> Las columnas de la base de datos se convierten en NULL automáticamente según sea necesario al usar la asignación TPH.

## <a name="data-annotations"></a>Anotaciones de datos

No se pueden usar anotaciones de datos para configurar la herencia.

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para configurar el nombre y el tipo de la columna discriminadora y los valores que se usan para identificar cada tipo en la jerarquía.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/InheritanceTPHDiscriminator.cs#Inheritance)]

## <a name="configuring-the-discriminator-property"></a>Configurar la propiedad Discriminator

En los ejemplos anteriores, el discriminador se crea como una [propiedad Shadow](xref:core/modeling/shadow-properties) en la entidad base de la jerarquía. Dado que es una propiedad del modelo, se puede configurar igual que otras propiedades. Por ejemplo, para establecer la longitud máxima cuando se usa el discriminador predeterminado, por Convención:

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

El discriminador también se puede asignar a una propiedad de CLR real en la entidad. Por ejemplo:

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

Combinando estas dos cosas, es posible asignar el discriminador a una propiedad real y configurarlo:

```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
