---
title: "Creación de un modelo - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
ms.technology: entity-framework-core
uid: core/modeling/index
ms.openlocfilehash: 1ad0f6891fbc8ba2e4d102cc9997f053a9dddb66
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
---
# <a name="creating-a-model"></a>Creación de un modelo

Entity Framework usa un conjunto de convenciones para compilar un modelo basado en la forma de las clases de entidad. Puede especificar una configuración adicional para complementar o reemplazar lo que se ha detectado por convención.

Este artículo trata de la configuración que se puede aplicar a un modelo para cualquier almacén de datos y que se puede aplicar al elegir como destino cualquier base de datos relacional. Los proveedores también pueden habilitar la configuración específica de un almacén de datos determinado. Para obtener documentación sobre la configuración específica del proveedor, vea la sección [Proveedores de bases de datos](../providers/index.md).

> [!TIP]  
> Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) de este artículo en GitHub.

## <a name="methods-of-configuration"></a>Métodos de configuración

### <a name="fluent-api"></a>API fluida

Puede reemplazar el método `OnModelCreating` del contexto derivado y usar `ModelBuilder API` para configurar el modelo. Este es el método más eficaz de configuración y permite especificar la configuración sin modificar las clases de entidad. La configuración de API fluida tiene la prioridad más alta y reemplaza las anotaciones de datos y las convenciones.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Required.cs?range=5-15&highlight=5-10)] -->

``` csharp
    class MyContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<Blog>()
                .Property(b => b.Url)
                .IsRequired();
        }
    }
```

### <a name="data-annotations"></a>Anotaciones de datos

También puede aplicar atributos (conocidos como anotaciones de datos) a las clases y las propiedades. Las anotaciones de datos reemplazan a las convenciones, pero son reemplazadas por la configuración de API fluida.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?range=11-16&highlight=4)] -->
``` csharp
    public class Blog
    {
        public int BlogId { get; set; }
        [Required]
        public string Url { get; set; }
    }
```
