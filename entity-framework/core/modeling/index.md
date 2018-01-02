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
# <a name="creating-a-model"></a><span data-ttu-id="347cd-102">Creación de un modelo</span><span class="sxs-lookup"><span data-stu-id="347cd-102">Creating a Model</span></span>

<span data-ttu-id="347cd-103">Entity Framework usa un conjunto de convenciones para compilar un modelo basado en la forma de las clases de entidad.</span><span class="sxs-lookup"><span data-stu-id="347cd-103">Entity Framework uses a set of conventions to build a model based on the shape of your entity classes.</span></span> <span data-ttu-id="347cd-104">Puede especificar una configuración adicional para complementar o reemplazar lo que se ha detectado por convención.</span><span class="sxs-lookup"><span data-stu-id="347cd-104">You can specify additional configuration to supplement and/or override what was discovered by convention.</span></span>

<span data-ttu-id="347cd-105">Este artículo trata de la configuración que se puede aplicar a un modelo para cualquier almacén de datos y que se puede aplicar al elegir como destino cualquier base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="347cd-105">This article covers configuration that can be applied to a model targeting any data store and that which can be applied when targeting any relational database.</span></span> <span data-ttu-id="347cd-106">Los proveedores también pueden habilitar la configuración específica de un almacén de datos determinado.</span><span class="sxs-lookup"><span data-stu-id="347cd-106">Providers may also enable configuration that is specific to a particular data store.</span></span> <span data-ttu-id="347cd-107">Para obtener documentación sobre la configuración específica del proveedor, vea la sección [Proveedores de bases de datos](../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="347cd-107">For documentation on provider specific configuration see the [Database Providers](../providers/index.md) section.</span></span>

> [!TIP]  
> <span data-ttu-id="347cd-108">Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) de este artículo en GitHub.</span><span class="sxs-lookup"><span data-stu-id="347cd-108">You can view this article’s [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) on GitHub.</span></span>

## <a name="methods-of-configuration"></a><span data-ttu-id="347cd-109">Métodos de configuración</span><span class="sxs-lookup"><span data-stu-id="347cd-109">Methods of configuration</span></span>

### <a name="fluent-api"></a><span data-ttu-id="347cd-110">API fluida</span><span class="sxs-lookup"><span data-stu-id="347cd-110">Fluent API</span></span>

<span data-ttu-id="347cd-111">Puede reemplazar el método `OnModelCreating` del contexto derivado y usar `ModelBuilder API` para configurar el modelo.</span><span class="sxs-lookup"><span data-stu-id="347cd-111">You can override the `OnModelCreating` method in your derived context and use the `ModelBuilder API` to configure your model.</span></span> <span data-ttu-id="347cd-112">Este es el método más eficaz de configuración y permite especificar la configuración sin modificar las clases de entidad.</span><span class="sxs-lookup"><span data-stu-id="347cd-112">This is the most powerful method of configuration and allows configuration to be specified without modifying your entity classes.</span></span> <span data-ttu-id="347cd-113">La configuración de API fluida tiene la prioridad más alta y reemplaza las anotaciones de datos y las convenciones.</span><span class="sxs-lookup"><span data-stu-id="347cd-113">Fluent API configuration has the highest precedence and will override conventions and data annotations.</span></span>

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

### <a name="data-annotations"></a><span data-ttu-id="347cd-114">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="347cd-114">Data Annotations</span></span>

<span data-ttu-id="347cd-115">También puede aplicar atributos (conocidos como anotaciones de datos) a las clases y las propiedades.</span><span class="sxs-lookup"><span data-stu-id="347cd-115">You can also apply attributes (known as Data Annotations) to your classes and properties.</span></span> <span data-ttu-id="347cd-116">Las anotaciones de datos reemplazan a las convenciones, pero son reemplazadas por la configuración de API fluida.</span><span class="sxs-lookup"><span data-stu-id="347cd-116">Data annotations will override conventions, but will be overwritten by Fluent API configuration.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?range=11-16&highlight=4)] -->
``` csharp
    public class Blog
    {
        public int BlogId { get; set; }
        [Required]
        public string Url { get; set; }
    }
```
