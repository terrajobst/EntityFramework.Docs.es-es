---
title: Vistas de asignación generados previamente - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 917ba9c8-6ddf-4631-ab8c-c4fb378c2fcd
ms.openlocfilehash: da5d59ba5a899a0ee3a1eec3db0da1b4ece871d8
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "46284114"
---
# <a name="pre-generated-mapping-views"></a>Vistas de asignación generados previamente
Antes de Entity Framework puede ejecutar una consulta o guardar los cambios en el origen de datos, debe generar un conjunto de vistas de asignación para tener acceso a la base de datos. Estas vistas de asignación son un conjunto de instrucción de Entity SQL que representan la base de datos de forma abstracta y forman parte de los metadatos que se almacena en caché por dominio de aplicación. Si crea varias instancias del mismo contexto en el mismo dominio de aplicación, volverá a usar las vistas de asignación de los metadatos almacenados en caché en lugar de volver a generarlos. Porque la generación de vistas de asignación es una parte significativa del costo total de la ejecución de la primera consulta, Entity Framework permite generar previamente las vistas de asignación e incluirlos en el proyecto compilado. Para obtener más información, consulte [consideraciones de rendimiento (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).

## <a name="generating-mapping-views-with-the-ef-power-tools"></a>Generar vistas con EF Power Tools de asignación

Para generar previamente las vistas más sencillo es usar el [EF Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d). Una vez que tenga instaladas las herramientas de alimentación tendrá una opción de menú para generar vistas, como sigue.

-   Para **Code First** modelos haga doble clic en el archivo de código que contiene la clase DbContext.
-   Para **EF Designer** modelos haga doble clic en el archivo EDMX.

![generar vistas](~/ef6/media/generateviews.png)

Una vez que finalice el proceso, tendrá una clase similar a lo siguiente genera

![Vistas generadas](~/ef6/media/generatedviews.png)

Ahora cuando se ejecuta la aplicación EF utilizará esta clase para cargar las vistas según sea necesario. Si cambia el modelo y no volver a generar la clase EF producirá una excepción.

## <a name="generating-mapping-views-from-code---ef6-onwards"></a>La generación de vistas de asignación de código - EF6 y versiones posteriores

La otra forma de generar vistas es usar las API que ofrece EF. Al usar este método que tiene la libertad para serializar las vistas pero desee, pero también tiene que cargar las vistas usted mismo.

> [!NOTE]
> **EF6 y versiones posteriores solo** -las API que se muestra en esta sección se introdujeron en Entity Framework 6. Si usa una versión anterior que no se aplica esta información.

### <a name="generating-views"></a>Generación de vistas

Las API para generar vistas están en la clase System.Data.Entity.Core.Mapping.StorageMappingItemCollection. Puede recuperar un StorageMappingCollection para un contexto mediante el MetadataWorkspace de un ObjectContext. Si usa la API de DbContext más reciente, a continuación, puede tener acceso a esta mediante el uso de la clase IObjectContextAdapter como aparece a continuación, en este código, tenemos una instancia de DbContext derivada llama a dbContext:

``` csharp
    var objectContext = ((IObjectContextAdapter) dbContext).ObjectContext;
    var  mappingCollection = (StorageMappingItemCollection)objectContext.MetadataWorkspace
                                                                        .GetItemCollection(DataSpace.CSSpace);
```

Una vez que StorageMappingItemCollection, a continuación, puede obtener acceso a los métodos GenerateViews y ComputeMappingHashValue.

``` csharp
    public Dictionary\<EntitySetBase, DbMappingView> GenerateViews(IList<EdmSchemaError> errors)
    public string ComputeMappingHashValue()
```

El primer método crea un diccionario con una entrada para cada vista en la asignación de contenedor. El segundo método calcula un valor hash para la asignación de contenedor único y se utiliza en tiempo de ejecución para validar que el modelo no ha cambiado desde que se han generado previamente las vistas. Invalidaciones de los dos métodos se proporcionan para escenarios complejos que implican varias asignaciones de contenedor.

Al generar las vistas que se llame al método GenerateViews y, a continuación, escribir el EntitySetBase y DbMappingView resultante. También deberá almacenar el hash generado por el método ComputeMappingHashValue.

### <a name="loading-views"></a>Vistas de carga

Para cargar las vistas generadas por el método GenerateViews, puede proporcionar EF con una clase que hereda de la clase abstracta DbMappingViewCache. DbMappingViewCache especifica dos métodos que debe implementar:

``` csharp
    public abstract string MappingHashValue { get; }
    public abstract DbMappingView GetView(EntitySetBase extent);
```

La propiedad MappingHashValue debe devolver el valor hash generado por el método ComputeMappingHashValue. Cuando EF se va a pedir las vistas primero generará y comparar el valor hash del modelo con el hash devuelto por esta propiedad. Si no coinciden con EF producirá una excepción EntityCommandCompilationException.

El método GetView aceptará un EntitySetBase y deberá devolver un DbMappingVIew que contiene el EntitySql que se generó para el que estaba asociado con el EntitySetBase determinado en el diccionario generado por el método GenerateViews. Si pide EF una vista que no tiene, a continuación, GetView debe devolver null.

El siguiente es un extracto de la DbMappingViewCache que se genera con las herramientas avanzadas, como se describió anteriormente, en la que vemos una manera de almacenar y recuperar el EntitySql necesario.

``` csharp
    public override string MappingHashValue
    {
        get { return "a0b843f03dd29abee99789e190a6fb70ce8e93dc97945d437d9a58fb8e2afd2e"; }
    }

    public override DbMappingView GetView(EntitySetBase extent)
    {
        if (extent == null)
        {
            throw new ArgumentNullException("extent");
        }

        var extentName = extent.EntityContainer.Name + "." + extent.Name;

        if (extentName == "BlogContext.Blogs")
        {
            return GetView2();
        }

        if (extentName == "BlogContext.Posts")
        {
            return GetView3();
        }

        return null;
    }

    private static DbMappingView GetView2()
    {
        return new DbMappingView(@"
            SELECT VALUE -- Constructing Blogs
            [BlogApp.Models.Blog](T1.Blog_BlogId, T1.Blog_Test, T1.Blog_title, T1.Blog_Active, T1.Blog_SomeDecimal)
            FROM (
            SELECT
                T.BlogId AS Blog_BlogId,
                T.Test AS Blog_Test,
                T.title AS Blog_title,
                T.Active AS Blog_Active,
                T.SomeDecimal AS Blog_SomeDecimal,
                True AS _from0
            FROM CodeFirstDatabase.Blog AS T
            ) AS T1");
    }
```

Para que use EF su DbMappingViewCache agrega utilice el DbMappingViewCacheTypeAttribute, especificar el contexto que se crearon para. En el código siguiente se asocia el BlogContext con la clase MyMappingViewCache.

``` csharp
    [assembly: DbMappingViewCacheType(typeof(BlogContext), typeof(MyMappingViewCache))]
```

Para escenarios más complejos, las instancias de caché de la vista asignación pueden proporcionarse mediante la especificación de un generador de caché de vista de asignación. Esto puede hacerse mediante la implementación de la clase abstracta System.Data.Entity.Infrastructure.MappingViews.DbMappingViewCacheFactory. La instancia de la fábrica de caché de vistas de asignación que se utiliza se puede recuperar o establecer mediante el StorageMappingItemCollection.MappingViewCacheFactoryproperty.
