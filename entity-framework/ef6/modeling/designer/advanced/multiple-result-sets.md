---
title: Procedimientos almacenados con varios conjuntos de resultados - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 1b3797f9-cd3d-4752-a55e-47b84b399dc1
ms.openlocfilehash: bb104ac5f584d26d279259a173de9afe3f018968
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996180"
---
# <a name="stored-procedures-with-multiple-result-sets"></a><span data-ttu-id="50ffc-102">Procedimientos almacenados con varios conjuntos de resultados</span><span class="sxs-lookup"><span data-stu-id="50ffc-102">Stored Procedures with Multiple Result Sets</span></span>
<span data-ttu-id="50ffc-103">En ocasiones, al usar almacenados establecen procedimientos deberá devolver más de un resultado.</span><span class="sxs-lookup"><span data-stu-id="50ffc-103">Sometimes when using stored procedures you will need to return more than one result set.</span></span> <span data-ttu-id="50ffc-104">Este escenario se usa habitualmente para reducir el número de la base de datos de ciclos de ida y necesarios para crear una sola pantalla.</span><span class="sxs-lookup"><span data-stu-id="50ffc-104">This scenario is commonly used to reduce the number of database round trips required to compose a single screen.</span></span> <span data-ttu-id="50ffc-105">Antes de EF5, Entity Framework permitiría que el procedimiento almacenado que se llame, pero solo devuelve el primer resultado establecido en el código de llamada.</span><span class="sxs-lookup"><span data-stu-id="50ffc-105">Prior to EF5, Entity Framework would allow the stored procedure to be called but would only return the first result set to the calling code.</span></span>

<span data-ttu-id="50ffc-106">En este artículo le mostrará dos maneras en que puede usar para tener acceso a más de un conjunto de resultados desde un procedimiento almacenado en Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="50ffc-106">This article will show you two ways that you can use to access more than one result set from a stored procedure in Entity Framework.</span></span> <span data-ttu-id="50ffc-107">Una que usa solo código y funciona con tanto código primero y EF Designer y otra que sólo funciona con EF Designer.</span><span class="sxs-lookup"><span data-stu-id="50ffc-107">One that uses just code and works with both Code first and the EF Designer and one that only works with the EF Designer.</span></span> <span data-ttu-id="50ffc-108">Las herramientas y soporte API para esto deben mejorar en futuras versiones de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="50ffc-108">The tooling and API support for this should improve in future versions of Entity Framework.</span></span>

## <a name="model"></a><span data-ttu-id="50ffc-109">Modelo</span><span class="sxs-lookup"><span data-stu-id="50ffc-109">Model</span></span>

<span data-ttu-id="50ffc-110">Los ejemplos de este artículo usan un Blog básico y modelo publicaciones donde un blog tiene todas las entradas y la entrada pertenece a un blog único.</span><span class="sxs-lookup"><span data-stu-id="50ffc-110">The examples in this article use a basic Blog and Posts model where a blog has many posts and a post belongs to a single blog.</span></span> <span data-ttu-id="50ffc-111">Usaremos un procedimiento almacenado en la base de datos que devuelve todos los blogs y publicaciones, algo parecido a esto:</span><span class="sxs-lookup"><span data-stu-id="50ffc-111">We will use a stored procedure in the database that returns all blogs and posts, something like this:</span></span>

``` SQL
    CREATE PROCEDURE [dbo].[GetAllBlogsAndPosts]
    AS
        SELECT * FROM dbo.Blogs
        SELECT * FROM dbo.Posts
```

## <a name="accessing-multiple-result-sets-with-code"></a><span data-ttu-id="50ffc-112">Acceso a los resultados de varios conjuntos con código</span><span class="sxs-lookup"><span data-stu-id="50ffc-112">Accessing Multiple Result Sets with Code</span></span>

<span data-ttu-id="50ffc-113">Podemos ejecutar código de uso para emitir un comando SQL sin procesar para ejecutar el procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="50ffc-113">We can execute use code to issue a raw SQL command to execute our stored procedure.</span></span> <span data-ttu-id="50ffc-114">La ventaja de este enfoque es que funciona con tanto código primero y EF Designer.</span><span class="sxs-lookup"><span data-stu-id="50ffc-114">The advantage of this approach is that it works with both Code first and the EF Designer.</span></span>

<span data-ttu-id="50ffc-115">Para obtener resultados múltiples establece el espacio de trabajo que es necesario colocar a la API de ObjectContext por mediante la interfaz de clase IObjectContextAdapter.</span><span class="sxs-lookup"><span data-stu-id="50ffc-115">In order to get multiple result sets working we need to drop to the ObjectContext API by using the IObjectContextAdapter interface.</span></span>

<span data-ttu-id="50ffc-116">Una vez que tenemos un ObjectContext, a continuación, podemos usar el método Translate para traducir los resultados de nuestro procedimiento almacenado en las entidades que pueden controlarse y utilizar en EF como normal.</span><span class="sxs-lookup"><span data-stu-id="50ffc-116">Once we have an ObjectContext then we can use the Translate method to translate the results of our stored procedure into entities that can be tracked and used in EF as normal.</span></span> <span data-ttu-id="50ffc-117">Ejemplo de código siguiente muestra esto en acción.</span><span class="sxs-lookup"><span data-stu-id="50ffc-117">The following code sample demonstrates this in action.</span></span>

``` csharp
    using (var db = new BloggingContext())
    {
        // If using Code First we need to make sure the model is built before we open the connection
        // This isn't required for models created with the EF Designer
        db.Database.Initialize(force: false);

        // Create a SQL command to execute the sproc
        var cmd = db.Database.Connection.CreateCommand();
        cmd.CommandText = "[dbo].[GetAllBlogsAndPosts]";

        try
        {

            db.Database.Connection.Open();
            // Run the sproc
            var reader = cmd.ExecuteReader();

            // Read Blogs from the first result set
            var blogs = ((IObjectContextAdapter)db)
                .ObjectContext
                .Translate<Blog>(reader, "Blogs", MergeOption.AppendOnly);   


            foreach (var item in blogs)
            {
                Console.WriteLine(item.Name);
            }        

            // Move to second result set and read Posts
            reader.NextResult();
            var posts = ((IObjectContextAdapter)db)
                .ObjectContext
                .Translate<Post>(reader, "Posts", MergeOption.AppendOnly);


            foreach (var item in posts)
            {
                Console.WriteLine(item.Title);
            }
        }
        finally
        {
            db.Database.Connection.Close();
        }
    }
```

<span data-ttu-id="50ffc-118">El método Translate acepta el lector que se ha recibido cuando se ejecutan el procedimiento, un nombre de EntitySet y un MergeOption.</span><span class="sxs-lookup"><span data-stu-id="50ffc-118">The Translate method accepts the reader that we received when we executed the procedure, an EntitySet name, and a MergeOption.</span></span> <span data-ttu-id="50ffc-119">El nombre de EntitySet será el mismo que la propiedad DbSet en su contexto derivado.</span><span class="sxs-lookup"><span data-stu-id="50ffc-119">The EntitySet name will be the same as the DbSet property on your derived context.</span></span> <span data-ttu-id="50ffc-120">La enumeración MergeOption controla cómo se controlan los resultados si la misma entidad ya existe en la memoria.</span><span class="sxs-lookup"><span data-stu-id="50ffc-120">The MergeOption enum controls how results are handled if the same entity already exists in memory.</span></span>

<span data-ttu-id="50ffc-121">Aquí se recorrer en iteración la colección de blogs antes de que llamamos NextResult, esto es importante, dado el código anterior porque el primer conjunto de resultados debe consumirse antes de pasar al siguiente conjunto de resultados.</span><span class="sxs-lookup"><span data-stu-id="50ffc-121">Here we iterate through the collection of blogs before we call NextResult, this is important given the above code because the first result set must be consumed before moving to the next result set.</span></span>

<span data-ttu-id="50ffc-122">Una vez que traducen los dos métodos se llaman, a continuación, las entidades Blog y Post se realiza un seguimiento por EF del mismo modo que cualquier otra entidad por lo que se puede modificar o eliminar y guarda como normal.</span><span class="sxs-lookup"><span data-stu-id="50ffc-122">Once the two translate methods are called then the Blog and Post entities are tracked by EF the same way as any other entity and so can be modified or deleted and saved as normal.</span></span>

>[!NOTE]
> <span data-ttu-id="50ffc-123">EF no tiene ninguna asignación en cuenta cuando crea entidades mediante el método Translate.</span><span class="sxs-lookup"><span data-stu-id="50ffc-123">EF does not take any mapping into account when it creates entities using the Translate method.</span></span> <span data-ttu-id="50ffc-124">Simplemente coincidirá con nombres de columna en el conjunto de resultados con los nombres de propiedad en las clases.</span><span class="sxs-lookup"><span data-stu-id="50ffc-124">It will simply match column names in the result set with property names on your classes.</span></span>

>[!NOTE]
> <span data-ttu-id="50ffc-125">Que si la carga diferida está habilitada, tiene acceso a la propiedad de publicaciones en una de las entidades blog, a continuación, EF se conecte a la base de datos para cargar de forma diferida todas las publicaciones, aunque ya se han cargado todas ellas.</span><span class="sxs-lookup"><span data-stu-id="50ffc-125">That if you have lazy loading enabled, accessing the posts property on one of the blog entities then EF will connect to the database to lazily load all posts, even though we have already loaded them all.</span></span> <span data-ttu-id="50ffc-126">Esto es porque EF no puede saber si han cargado todas las publicaciones o si hay más en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="50ffc-126">This is because EF cannot know whether or not you have loaded all posts or if there are more in the database.</span></span> <span data-ttu-id="50ffc-127">Si desea evitar este problema, deberá deshabilitar la carga diferida.</span><span class="sxs-lookup"><span data-stu-id="50ffc-127">If you want to avoid this then you will need to disable lazy loading.</span></span>

## <a name="multiple-result-sets-with-configured-in-edmx"></a><span data-ttu-id="50ffc-128">Varios conjuntos de resultados con configurado en EDMX</span><span class="sxs-lookup"><span data-stu-id="50ffc-128">Multiple Result Sets with Configured in EDMX</span></span>

>[!NOTE]
> <span data-ttu-id="50ffc-129">Debe tener como destino .NET Framework 4.5 para que pueda configurar varios conjuntos de resultados en EDMX.</span><span class="sxs-lookup"><span data-stu-id="50ffc-129">You must target .NET Framework 4.5 to be able to configure multiple result sets in EDMX.</span></span> <span data-ttu-id="50ffc-130">Si tiene como destino .NET 4.0, puede usar el método basado en código que se muestra en la sección anterior.</span><span class="sxs-lookup"><span data-stu-id="50ffc-130">If you are targeting .NET 4.0 you can use the code-based method shown in the previous section.</span></span>

<span data-ttu-id="50ffc-131">Si usa el Diseñador de EF, también puede modificar el modelo para que sepa sobre los distintos conjuntos de resultados que se devolverán.</span><span class="sxs-lookup"><span data-stu-id="50ffc-131">If you are using the EF Designer, you can also modify your model so that it knows about the different result sets that will be returned.</span></span> <span data-ttu-id="50ffc-132">Algo que debe conocer antes de mano es que las herramientas no son resultados múltiples establece compatible con, por lo que deberá modificar manualmente el archivo edmx.</span><span class="sxs-lookup"><span data-stu-id="50ffc-132">One thing to know before hand is that the tooling is not multiple result set aware, so you will need to manually edit the edmx file.</span></span> <span data-ttu-id="50ffc-133">Editar el archivo edmx que esto funcionará, pero también interrumpirá la validación del modelo en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="50ffc-133">Editing the edmx file like this will work but it will also break the validation of the model in VS.</span></span> <span data-ttu-id="50ffc-134">Por lo que si la validación del modelo siempre obtendrá errores.</span><span class="sxs-lookup"><span data-stu-id="50ffc-134">So if you validate your model you will always get errors.</span></span>

-   <span data-ttu-id="50ffc-135">Para ello, deberá agregar el procedimiento almacenado al modelo como lo haría para una consulta de conjunto de resultados único.</span><span class="sxs-lookup"><span data-stu-id="50ffc-135">In order to do this you need to add the stored procedure to your model as you would for a single result set query.</span></span>
-   <span data-ttu-id="50ffc-136">Una vez que esto tiene que haga clic con el botón derecho en el modelo y seleccione **abrir con...**</span><span class="sxs-lookup"><span data-stu-id="50ffc-136">Once you have this then you need to right click on your model and select **Open With..**</span></span> <span data-ttu-id="50ffc-137">a continuación, **Xml**</span><span class="sxs-lookup"><span data-stu-id="50ffc-137">then **Xml**</span></span>

    ![OpenAs](~/ef6/media/openas.png)

<span data-ttu-id="50ffc-139">Una vez que tenga el modelo que se abrió como XML, deberá hacer lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="50ffc-139">Once you have the model opened as XML then you need to do the following steps:</span></span>

-   <span data-ttu-id="50ffc-140">Busque la importación de función y de tipo compleja en el modelo:</span><span class="sxs-lookup"><span data-stu-id="50ffc-140">Find the complex type and function import in your model:</span></span>

``` xml
    <!-- CSDL content -->
    <edmx:ConceptualModels>

    ...

      <FunctionImport Name="GetAllBlogsAndPosts" ReturnType="Collection(BlogModel.GetAllBlogsAndPosts_Result)" />

    ...

      <ComplexType Name="GetAllBlogsAndPosts_Result">
        <Property Type="Int32" Name="BlogId" Nullable="false" />
        <Property Type="String" Name="Name" Nullable="false" MaxLength="255" />
        <Property Type="String" Name="Description" Nullable="true" />
      </ComplexType>

    ...

    </edmx:ConceptualModels>
```

 

-   <span data-ttu-id="50ffc-141">Quitar el tipo complejo</span><span class="sxs-lookup"><span data-stu-id="50ffc-141">Remove the complex type</span></span>
-   <span data-ttu-id="50ffc-142">Actualizar la importación de función para que asigna a las entidades, en nuestro caso que tendrá un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="50ffc-142">Update the function import so that it maps to your entities, in our case it will look like the following:</span></span>

``` xml
    <FunctionImport Name="GetAllBlogsAndPosts">
      <ReturnType EntitySet="Blogs" Type="Collection(BlogModel.Blog)" />
      <ReturnType EntitySet="Posts" Type="Collection(BlogModel.Post)" />
    </FunctionImport>
```

<span data-ttu-id="50ffc-143">Esto indica que el modelo que el procedimiento almacenado devolverá dos colecciones, una de las entradas de blog y una de las entradas de la publicación.</span><span class="sxs-lookup"><span data-stu-id="50ffc-143">This tells the model that the stored procedure will return two collections, one of blog entries and one of post entries.</span></span>

-   <span data-ttu-id="50ffc-144">Busque el elemento de asignación de función:</span><span class="sxs-lookup"><span data-stu-id="50ffc-144">Find the function mapping element:</span></span>

``` xml
    <!-- C-S mapping content -->
    <edmx:Mappings>

    ...

      <FunctionImportMapping FunctionImportName="GetAllBlogsAndPosts" FunctionName="BlogModel.Store.GetAllBlogsAndPosts">
        <ResultMapping>
          <ComplexTypeMapping TypeName="BlogModel.GetAllBlogsAndPosts_Result">
            <ScalarProperty Name="BlogId" ColumnName="BlogId" />
            <ScalarProperty Name="Name" ColumnName="Name" />
            <ScalarProperty Name="Description" ColumnName="Description" />
          </ComplexTypeMapping>
        </ResultMapping>
      </FunctionImportMapping>

    ...

    </edmx:Mappings>
```

-   <span data-ttu-id="50ffc-145">Reemplace la asignación de resultado por uno para cada entidad que se devuelve como el siguiente:</span><span class="sxs-lookup"><span data-stu-id="50ffc-145">Replace the result mapping with one for each entity being returned, such as the following:</span></span>

``` xml
    <ResultMapping>
      <EntityTypeMapping TypeName ="BlogModel.Blog">
        <ScalarProperty Name="BlogId" ColumnName="BlogId" />
        <ScalarProperty Name="Name" ColumnName="Name" />
        <ScalarProperty Name="Description" ColumnName="Description" />
      </EntityTypeMapping>
    </ResultMapping>
    <ResultMapping>
      <EntityTypeMapping TypeName="BlogModel.Post">
        <ScalarProperty Name="BlogId" ColumnName="BlogId" />
        <ScalarProperty Name="PostId" ColumnName="PostId"/>
        <ScalarProperty Name="Title" ColumnName="Title" />
        <ScalarProperty Name="Text" ColumnName="Text" />
      </EntityTypeMapping>
    </ResultMapping>
```

<span data-ttu-id="50ffc-146">También es posible asignar los conjuntos de resultados a los tipos complejos, como el que se crean de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="50ffc-146">It is also possible to map the result sets to complex types, such as the one created by default.</span></span> <span data-ttu-id="50ffc-147">Para ello cree un nuevo tipo complejo, en lugar de quitarlos y usar los tipos complejos en cualquier lugar que hubiera usado los nombres de entidad en los ejemplos anteriores.</span><span class="sxs-lookup"><span data-stu-id="50ffc-147">To do this you create a new complex type, instead of removing them, and use the complex types everywhere that you had used the entity names in the examples above.</span></span>

<span data-ttu-id="50ffc-148">Una vez que se han cambiado estas asignaciones, a continuación, puede guardar el modelo y ejecute el siguiente código para usar el procedimiento almacenado:</span><span class="sxs-lookup"><span data-stu-id="50ffc-148">Once these mappings have been changed then you can save the model and execute the following code to use the stored procedure:</span></span>

``` csharp
    using (var db = new BlogEntities())
    {
        var results = db.GetAllBlogsAndPosts();

        foreach (var result in results)
        {
            Console.WriteLine("Blog: " + result.Name);
        }

        var posts = results.GetNextResult<Post>();

        foreach (var result in posts)
        {
            Console.WriteLine("Post: " + result.Title);
        }

        Console.ReadLine();
    }
```

>[!NOTE]
> <span data-ttu-id="50ffc-149">Si edita manualmente el archivo edmx para el modelo se sobrescribirán si alguna vez se regenera el modelo de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="50ffc-149">If you manually edit the edmx file for your model it will be overwritten if you ever regenerate the model from the database.</span></span>

## <a name="summary"></a><span data-ttu-id="50ffc-150">Resumen</span><span class="sxs-lookup"><span data-stu-id="50ffc-150">Summary</span></span>

<span data-ttu-id="50ffc-151">Aquí hemos mostrado dos métodos diferentes de obtener acceso a varios resultados se establece mediante Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="50ffc-151">Here we have shown two different methods of accessing multiple result sets using Entity Framework.</span></span> <span data-ttu-id="50ffc-152">Ambos son igualmente válidos según la situación y las preferencias y debe elegir lo que parece más adecuado para sus circunstancias.</span><span class="sxs-lookup"><span data-stu-id="50ffc-152">Both of them are equally valid depending on your situation and preferences and you should choose the one that seems best for your circumstances.</span></span> <span data-ttu-id="50ffc-153">Está previsto que la compatibilidad con resultados múltiples conjuntos de serán mejorado en futuras versiones de Entity Framework y que realizar los pasos de este documento ya no será necesario.</span><span class="sxs-lookup"><span data-stu-id="50ffc-153">It is planned that the support for multiple result sets will be improved in future versions of Entity Framework and that performing the steps in this document will no longer be necessary.</span></span>
