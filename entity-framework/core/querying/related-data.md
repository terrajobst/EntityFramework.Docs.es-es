---
title: Carga los datos - Core EF relacionados
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: cd26bd2e6f85083f73d97b1356d0ba38f53e0b8f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
---
# <a name="loading-related-data"></a>Cargar datos relacionados

Entity Framework Core le permite usar las propiedades de navegación en el modelo para cargar las entidades relacionadas. Hay tres patrones O/RM comunes usados para cargar los datos relacionados.
* **Carga diligente** significa que los datos relacionados se cargan desde la base de datos como parte de la consulta inicial.
* **Carga explícita** significa que los datos relacionados se cargan explícitamente desde la base de datos en un momento posterior.
* **Carga diferida** significa que los datos relacionados se cargan transparente desde la base de datos cuando se tiene acceso a la propiedad de navegación. Carga diferida aún no es posible con EF principales.

> [!TIP]  
> Puede ver este artículo [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) en GitHub.

## <a name="eager-loading"></a>Carga diligente

Puede usar el `Include` método para especificar los datos relacionados que se incluirá en los resultados de la consulta. En el ejemplo siguiente, tendrá los blogs que se devuelven en los resultados de sus `Posts` propiedad que se rellene con las entradas relacionadas.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> Propiedades de navegación automáticamente revisión de seguridad de entidad Framework Core will a cualquier otra entidad que se cargaron previamente en la instancia de contexto. Por lo que incluso si no se incluye explícitamente los datos de una propiedad de navegación, la propiedad también puede rellenar si algunas o todas las entidades relacionadas se cargaron previamente.


Puede incluir datos relacionados de varias relaciones en una sola consulta.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a>Incluidos varios niveles

Puede explorar en profundidad a través de relaciones para incluir varios niveles de datos relacionados con el `ThenInclude` método. En el ejemplo siguiente se cargan todos los blogs, sus entradas relacionadas y el autor de cada publicación.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

Se pueden encadenar varias llamadas a `ThenInclude` para continuar incluso más niveles de los datos relacionados.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

Puede combinar todo esto debe incluir datos relacionados de varios niveles y varias raíces en la misma consulta.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

Puede incluir varias entidades relacionadas para una de las entidades que se incluye. Por ejemplo, al consultar `Blog`s, incluye `Posts` y, a continuación, se van a incluir tanto el `Author` y `Tags` de la `Posts`. Para ello, debe especificar cada uno de ellos incluye empezando por la raíz de la ruta de acceso. Por ejemplo, `Blog -> Posts -> Author` y `Blog -> Posts -> Tags`. Esto no significa que obtendrá combinaciones redundantes, en la mayoría de los casos que se consolidará EF las combinaciones cuando la generación de SQL.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="ignored-includes"></a>Omite incluye

Si cambia la consulta para que ya no devuelve instancias del tipo de entidad que la consulta comienza con, se omiten los operadores de inclusión.

En el ejemplo siguiente, los operadores de inclusión se basan en el `Blog`, pero, a continuación, el `Select` operador se utiliza para cambiar la consulta para devolver un tipo anónimo. En este caso, los operadores de inclusión no tienen ningún efecto.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

De forma predeterminada, el núcleo de EF registrará una advertencia cuando se incluyen operadores se omiten. Vea [registro](../miscellaneous/logging.md) para obtener más información acerca de cómo ver los resultados del registro. Puede cambiar el comportamiento cuando un operador de inclusión se omite para producir o no hacer nada. Esto se realiza al configurar las opciones para el contexto - normalmente en `DbContext.OnConfiguring`, o bien en `Startup.cs` si utiliza ASP.NET Core.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a>Carga explícita

> [!NOTE]  
> Esta característica se introdujo en EF Core 1.1.

Puede cargar explícitamente una propiedad de navegación a través de la `DbContext.Entry(...)` API.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

Puede cargar explícitamente una propiedad de navegación mediante la ejecución de una consulta independiente que devuelve las entidades relacionadas. Si está habilitado el seguimiento de cambios, al cargar una entidad, Core EF automáticamente establecer las propiedades de navegación de la entitiy recientemente cargados para hacer referencia a una entidad que ya están cargada y establecer las propiedades de navegación de las entidades ya cargado para hacer referencia a la entidad recién cargado.

### <a name="querying-related-entities"></a>Consultar las entidades relacionadas

También puede obtener una consulta LINQ que representa el contenido de una propiedad de navegación.

Esto le permite hacer cosas como la ejecución de un operador agregado sobre las entidades relacionadas sin cargarlos en la memoria.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

También puede filtrar las entidades relacionadas se cargan en memoria.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a>Carga diferida

Carga diferida no es compatible todavía con EF Core. Puede ver el [elemento de la carga diferida en nuestro trabajo pendiente](https://github.com/aspnet/EntityFramework/issues/3797) para realizar el seguimiento de esta característica.

## <a name="related-data-and-serialization"></a>Serialización y los datos relacionados

Porque las propiedades de navegación de corrección telefónico automáticamente de will de EF Core, puede acabar con ciclos en el gráfico de objetos. Por ejemplo, cargando un blog y está relacionado con entradas dará como resultado un objeto de blog que hace referencia a una colección de entradas. Cada una de esas entradas tendrá una referencia al blog.

Algunos marcos de serialización no permiten estos ciclos. Por ejemplo, Json.NET producirá la excepción siguiente si un ciclo es generará.

> Newtonsoft.Json.JsonSerializationException: Self que hacen referencia a bucle detectado para la propiedad 'Blog' con el tipo 'MyApplication.Models.Blog'.

Si utiliza ASP.NET Core, puede configurar Json.NET para omitir los ciclos que encuentran en el gráfico de objetos. Esto se hace en el `ConfigureServices(...)` método `Startup.cs`.

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddMvc()
        .AddJsonOptions(
            options => options.SerializerSettings.ReferenceLoopHandling = Newtonsoft.Json.ReferenceLoopHandling.Ignore
        );

    ...
}
```
