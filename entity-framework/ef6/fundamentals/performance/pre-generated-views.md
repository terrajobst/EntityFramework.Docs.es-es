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
# <a name="pre-generated-mapping-views"></a><span data-ttu-id="f47d0-102">Vistas de asignación generados previamente</span><span class="sxs-lookup"><span data-stu-id="f47d0-102">Pre-generated mapping views</span></span>
<span data-ttu-id="f47d0-103">Antes de Entity Framework puede ejecutar una consulta o guardar los cambios en el origen de datos, debe generar un conjunto de vistas de asignación para tener acceso a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="f47d0-103">Before the Entity Framework can execute a query or save changes to the data source, it must generate a set of mapping views to access the database.</span></span> <span data-ttu-id="f47d0-104">Estas vistas de asignación son un conjunto de instrucción de Entity SQL que representan la base de datos de forma abstracta y forman parte de los metadatos que se almacena en caché por dominio de aplicación.</span><span class="sxs-lookup"><span data-stu-id="f47d0-104">These mapping views are a set of Entity SQL statement that represent the database in an abstract way, and are part of the metadata which is cached per application domain.</span></span> <span data-ttu-id="f47d0-105">Si crea varias instancias del mismo contexto en el mismo dominio de aplicación, volverá a usar las vistas de asignación de los metadatos almacenados en caché en lugar de volver a generarlos.</span><span class="sxs-lookup"><span data-stu-id="f47d0-105">If you create multiple instances of the same context in the same application domain, they will reuse mapping views from the cached metadata rather than regenerating them.</span></span> <span data-ttu-id="f47d0-106">Porque la generación de vistas de asignación es una parte significativa del costo total de la ejecución de la primera consulta, Entity Framework permite generar previamente las vistas de asignación e incluirlos en el proyecto compilado.</span><span class="sxs-lookup"><span data-stu-id="f47d0-106">Because mapping view generation is a significant part of the overall cost of executing the first query, the Entity Framework enables you to pre-generate mapping views and include them in the compiled project.</span></span> <span data-ttu-id="f47d0-107">Para obtener más información, consulte [consideraciones de rendimiento (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).</span><span class="sxs-lookup"><span data-stu-id="f47d0-107">For more information, see  [Performance Considerations (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).</span></span>

## <a name="generating-mapping-views-with-the-ef-power-tools"></a><span data-ttu-id="f47d0-108">Generar vistas con EF Power Tools de asignación</span><span class="sxs-lookup"><span data-stu-id="f47d0-108">Generating Mapping Views with the EF Power Tools</span></span>

<span data-ttu-id="f47d0-109">Para generar previamente las vistas más sencillo es usar el [EF Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d).</span><span class="sxs-lookup"><span data-stu-id="f47d0-109">The easiest way to pre-generate views is to use the [EF Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d).</span></span> <span data-ttu-id="f47d0-110">Una vez que tenga instaladas las herramientas de alimentación tendrá una opción de menú para generar vistas, como sigue.</span><span class="sxs-lookup"><span data-stu-id="f47d0-110">Once you have the Power Tools installed you will have a menu option to Generate Views, as below.</span></span>

-   <span data-ttu-id="f47d0-111">Para **Code First** modelos haga doble clic en el archivo de código que contiene la clase DbContext.</span><span class="sxs-lookup"><span data-stu-id="f47d0-111">For **Code First** models right-click on the code file that contains your DbContext class.</span></span>
-   <span data-ttu-id="f47d0-112">Para **EF Designer** modelos haga doble clic en el archivo EDMX.</span><span class="sxs-lookup"><span data-stu-id="f47d0-112">For **EF Designer** models right-click on your EDMX file.</span></span>

![generar vistas](~/ef6/media/generateviews.png)

<span data-ttu-id="f47d0-114">Una vez que finalice el proceso, tendrá una clase similar a lo siguiente genera</span><span class="sxs-lookup"><span data-stu-id="f47d0-114">Once the process is finished you will have a class similar to the following generated</span></span>

![Vistas generadas](~/ef6/media/generatedviews.png)

<span data-ttu-id="f47d0-116">Ahora cuando se ejecuta la aplicación EF utilizará esta clase para cargar las vistas según sea necesario.</span><span class="sxs-lookup"><span data-stu-id="f47d0-116">Now when you run your application EF will use this class to load views as required.</span></span> <span data-ttu-id="f47d0-117">Si cambia el modelo y no volver a generar la clase EF producirá una excepción.</span><span class="sxs-lookup"><span data-stu-id="f47d0-117">If your model changes and you do not re-generate this class then EF will throw an exception.</span></span>

## <a name="generating-mapping-views-from-code---ef6-onwards"></a><span data-ttu-id="f47d0-118">La generación de vistas de asignación de código - EF6 y versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="f47d0-118">Generating Mapping Views from Code - EF6 Onwards</span></span>

<span data-ttu-id="f47d0-119">La otra forma de generar vistas es usar las API que ofrece EF.</span><span class="sxs-lookup"><span data-stu-id="f47d0-119">The other way to generate views is to use the APIs that EF provides.</span></span> <span data-ttu-id="f47d0-120">Al usar este método que tiene la libertad para serializar las vistas pero desee, pero también tiene que cargar las vistas usted mismo.</span><span class="sxs-lookup"><span data-stu-id="f47d0-120">When using this method you have the freedom to serialize the views however you like, but you also need to load the views yourself.</span></span>

> [!NOTE]
> <span data-ttu-id="f47d0-121">**EF6 y versiones posteriores solo** -las API que se muestra en esta sección se introdujeron en Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="f47d0-121">**EF6 Onwards Only** - The APIs shown in this section were introduced in Entity Framework 6.</span></span> <span data-ttu-id="f47d0-122">Si usa una versión anterior que no se aplica esta información.</span><span class="sxs-lookup"><span data-stu-id="f47d0-122">If you are using an earlier version this information does not apply.</span></span>

### <a name="generating-views"></a><span data-ttu-id="f47d0-123">Generación de vistas</span><span class="sxs-lookup"><span data-stu-id="f47d0-123">Generating Views</span></span>

<span data-ttu-id="f47d0-124">Las API para generar vistas están en la clase System.Data.Entity.Core.Mapping.StorageMappingItemCollection.</span><span class="sxs-lookup"><span data-stu-id="f47d0-124">The APIs to generate views are on the System.Data.Entity.Core.Mapping.StorageMappingItemCollection class.</span></span> <span data-ttu-id="f47d0-125">Puede recuperar un StorageMappingCollection para un contexto mediante el MetadataWorkspace de un ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="f47d0-125">You can retrieve a StorageMappingCollection for a Context by using the MetadataWorkspace of an ObjectContext.</span></span> <span data-ttu-id="f47d0-126">Si usa la API de DbContext más reciente, a continuación, puede tener acceso a esta mediante el uso de la clase IObjectContextAdapter como aparece a continuación, en este código, tenemos una instancia de DbContext derivada llama a dbContext:</span><span class="sxs-lookup"><span data-stu-id="f47d0-126">If you are using the newer DbContext API then you can access this by using the IObjectContextAdapter like below, in this code we have an instance of your derived DbContext called dbContext:</span></span>

``` csharp
    var objectContext = ((IObjectContextAdapter) dbContext).ObjectContext;
    var  mappingCollection = (StorageMappingItemCollection)objectContext.MetadataWorkspace
                                                                        .GetItemCollection(DataSpace.CSSpace);
```

<span data-ttu-id="f47d0-127">Una vez que StorageMappingItemCollection, a continuación, puede obtener acceso a los métodos GenerateViews y ComputeMappingHashValue.</span><span class="sxs-lookup"><span data-stu-id="f47d0-127">Once you have the StorageMappingItemCollection then you can get access to the GenerateViews and ComputeMappingHashValue methods.</span></span>

``` csharp
    public Dictionary\<EntitySetBase, DbMappingView> GenerateViews(IList<EdmSchemaError> errors)
    public string ComputeMappingHashValue()
```

<span data-ttu-id="f47d0-128">El primer método crea un diccionario con una entrada para cada vista en la asignación de contenedor.</span><span class="sxs-lookup"><span data-stu-id="f47d0-128">The first method creates a dictionary with an entry for each view in the container mapping.</span></span> <span data-ttu-id="f47d0-129">El segundo método calcula un valor hash para la asignación de contenedor único y se utiliza en tiempo de ejecución para validar que el modelo no ha cambiado desde que se han generado previamente las vistas.</span><span class="sxs-lookup"><span data-stu-id="f47d0-129">The second method computes a hash value for the single container mapping and is used at runtime to validate that the model has not changed since the views were pre-generated.</span></span> <span data-ttu-id="f47d0-130">Invalidaciones de los dos métodos se proporcionan para escenarios complejos que implican varias asignaciones de contenedor.</span><span class="sxs-lookup"><span data-stu-id="f47d0-130">Overrides of the two methods are provided for complex scenarios involving multiple container mappings.</span></span>

<span data-ttu-id="f47d0-131">Al generar las vistas que se llame al método GenerateViews y, a continuación, escribir el EntitySetBase y DbMappingView resultante.</span><span class="sxs-lookup"><span data-stu-id="f47d0-131">When generating views you will call the GenerateViews method and then write out the resulting EntitySetBase and DbMappingView.</span></span> <span data-ttu-id="f47d0-132">También deberá almacenar el hash generado por el método ComputeMappingHashValue.</span><span class="sxs-lookup"><span data-stu-id="f47d0-132">You will also need to store the hash generated by the ComputeMappingHashValue method.</span></span>

### <a name="loading-views"></a><span data-ttu-id="f47d0-133">Vistas de carga</span><span class="sxs-lookup"><span data-stu-id="f47d0-133">Loading Views</span></span>

<span data-ttu-id="f47d0-134">Para cargar las vistas generadas por el método GenerateViews, puede proporcionar EF con una clase que hereda de la clase abstracta DbMappingViewCache.</span><span class="sxs-lookup"><span data-stu-id="f47d0-134">In order to load the views generated by the GenerateViews method, you can provide EF with a class that inherits from the DbMappingViewCache abstract class.</span></span> <span data-ttu-id="f47d0-135">DbMappingViewCache especifica dos métodos que debe implementar:</span><span class="sxs-lookup"><span data-stu-id="f47d0-135">DbMappingViewCache specifies two methods that you must implement:</span></span>

``` csharp
    public abstract string MappingHashValue { get; }
    public abstract DbMappingView GetView(EntitySetBase extent);
```

<span data-ttu-id="f47d0-136">La propiedad MappingHashValue debe devolver el valor hash generado por el método ComputeMappingHashValue.</span><span class="sxs-lookup"><span data-stu-id="f47d0-136">The MappingHashValue property must return the hash generated by the ComputeMappingHashValue method.</span></span> <span data-ttu-id="f47d0-137">Cuando EF se va a pedir las vistas primero generará y comparar el valor hash del modelo con el hash devuelto por esta propiedad.</span><span class="sxs-lookup"><span data-stu-id="f47d0-137">When EF is going to ask for views it will first generate and compare the hash value of the model with the hash returned by this property.</span></span> <span data-ttu-id="f47d0-138">Si no coinciden con EF producirá una excepción EntityCommandCompilationException.</span><span class="sxs-lookup"><span data-stu-id="f47d0-138">If they do not match then EF will throw an EntityCommandCompilationException exception.</span></span>

<span data-ttu-id="f47d0-139">El método GetView aceptará un EntitySetBase y deberá devolver un DbMappingVIew que contiene el EntitySql que se generó para el que estaba asociado con el EntitySetBase determinado en el diccionario generado por el método GenerateViews.</span><span class="sxs-lookup"><span data-stu-id="f47d0-139">The GetView method will accept an EntitySetBase and you need to return a DbMappingVIew containing the EntitySql that was generated for that was associated with the given EntitySetBase in the dictionary generated by the GenerateViews method.</span></span> <span data-ttu-id="f47d0-140">Si pide EF una vista que no tiene, a continuación, GetView debe devolver null.</span><span class="sxs-lookup"><span data-stu-id="f47d0-140">If EF asks for a view that you do not have then GetView should return null.</span></span>

<span data-ttu-id="f47d0-141">El siguiente es un extracto de la DbMappingViewCache que se genera con las herramientas avanzadas, como se describió anteriormente, en la que vemos una manera de almacenar y recuperar el EntitySql necesario.</span><span class="sxs-lookup"><span data-stu-id="f47d0-141">The following is an extract from the DbMappingViewCache that is generated with the Power Tools as described above, in it we see one way to store and retrieve the EntitySql required.</span></span>

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

<span data-ttu-id="f47d0-142">Para que use EF su DbMappingViewCache agrega utilice el DbMappingViewCacheTypeAttribute, especificar el contexto que se crearon para.</span><span class="sxs-lookup"><span data-stu-id="f47d0-142">To have EF use your DbMappingViewCache you add use the DbMappingViewCacheTypeAttribute, specifying the context that it was created for.</span></span> <span data-ttu-id="f47d0-143">En el código siguiente se asocia el BlogContext con la clase MyMappingViewCache.</span><span class="sxs-lookup"><span data-stu-id="f47d0-143">In the code below we associate the BlogContext with the MyMappingViewCache class.</span></span>

``` csharp
    [assembly: DbMappingViewCacheType(typeof(BlogContext), typeof(MyMappingViewCache))]
```

<span data-ttu-id="f47d0-144">Para escenarios más complejos, las instancias de caché de la vista asignación pueden proporcionarse mediante la especificación de un generador de caché de vista de asignación.</span><span class="sxs-lookup"><span data-stu-id="f47d0-144">For more complex scenarios, mapping view cache instances can be provided by specifying a mapping view cache factory.</span></span> <span data-ttu-id="f47d0-145">Esto puede hacerse mediante la implementación de la clase abstracta System.Data.Entity.Infrastructure.MappingViews.DbMappingViewCacheFactory.</span><span class="sxs-lookup"><span data-stu-id="f47d0-145">This can be done by implementing the abstract class System.Data.Entity.Infrastructure.MappingViews.DbMappingViewCacheFactory.</span></span> <span data-ttu-id="f47d0-146">La instancia de la fábrica de caché de vistas de asignación que se utiliza se puede recuperar o establecer mediante el StorageMappingItemCollection.MappingViewCacheFactoryproperty.</span><span class="sxs-lookup"><span data-stu-id="f47d0-146">The instance of the mapping view cache factory that is used can be retrieved or set using the StorageMappingItemCollection.MappingViewCacheFactoryproperty.</span></span>
