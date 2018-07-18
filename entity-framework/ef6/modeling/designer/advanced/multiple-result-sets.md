---
title: Procedimientos almacenados con varios conjuntos de resultados - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 1b3797f9-cd3d-4752-a55e-47b84b399dc1
caps.latest.revision: 3
ms.openlocfilehash: 68d544b0c553868ad1ff36cd24db19cff08db073
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2018
ms.locfileid: "39122391"
---
# <a name="stored-procedures-with-multiple-result-sets"></a>Procedimientos almacenados con varios conjuntos de resultados
En ocasiones, al usar almacenados establecen procedimientos deberá devolver más de un resultado. Este escenario se usa habitualmente para reducir el número de la base de datos de ciclos de ida y necesarios para crear una sola pantalla. Antes de EF5, Entity Framework permitiría que el procedimiento almacenado que se llame, pero solo devuelve el primer resultado establecido en el código de llamada.

En este artículo le mostrará dos maneras en que puede usar para tener acceso a más de un conjunto de resultados desde un procedimiento almacenado en Entity Framework. Una que usa solo código y funciona con tanto código primero y EF Designer y otra que sólo funciona con EF Designer. Las herramientas y soporte API para esto deben mejorar en futuras versiones de Entity Framework.

## <a name="model"></a>Modelo

Los ejemplos de este artículo usan un Blog básico y modelo publicaciones donde un blog tiene todas las entradas y la entrada pertenece a un blog único. Usaremos un procedimiento almacenado en la base de datos que devuelve todos los blogs y publicaciones, algo parecido a esto:

``` SQL
    CREATE PROCEDURE [dbo].[GetAllBlogsAndPosts]
    AS
        SELECT * FROM dbo.Blogs
        SELECT * FROM dbo.Posts
```

## <a name="accessing-multiple-result-sets-with-code"></a>Acceso a los resultados de varios conjuntos con código

Podemos ejecutar código de uso para emitir un comando SQL sin procesar para ejecutar el procedimiento almacenado. La ventaja de este enfoque es que funciona con tanto código primero y EF Designer.

Para obtener resultados múltiples establece el espacio de trabajo que es necesario colocar a la API de ObjectContext por mediante la interfaz de clase IObjectContextAdapter.

Una vez que tenemos un ObjectContext, a continuación, podemos usar el método Translate para traducir los resultados de nuestro procedimiento almacenado en las entidades que pueden controlarse y utilizar en EF como normal. Ejemplo de código siguiente muestra esto en acción.

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

El método Translate acepta el lector que se ha recibido cuando se ejecutan el procedimiento, un nombre de EntitySet y un MergeOption. El nombre de EntitySet será el mismo que la propiedad DbSet en su contexto derivado. La enumeración MergeOption controla cómo se controlan los resultados si la misma entidad ya existe en la memoria.

Aquí se recorrer en iteración la colección de blogs antes de que llamamos NextResult, esto es importante, dado el código anterior porque el primer conjunto de resultados debe consumirse antes de pasar al siguiente conjunto de resultados.

Una vez que traducen los dos métodos se llaman, a continuación, las entidades Blog y Post se realiza un seguimiento por EF del mismo modo que cualquier otra entidad por lo que se puede modificar o eliminar y guarda como normal.

>[!NOTE]
> EF no tiene ninguna asignación en cuenta cuando crea entidades mediante el método Translate. Simplemente coincidirá con nombres de columna en el conjunto de resultados con los nombres de propiedad en las clases.

>[!NOTE]
> Que si la carga diferida está habilitada, tiene acceso a la propiedad de publicaciones en una de las entidades blog, a continuación, EF se conecte a la base de datos para cargar de forma diferida todas las publicaciones, aunque ya se han cargado todas ellas. Esto es porque EF no puede saber si han cargado todas las publicaciones o si hay más en la base de datos. Si desea evitar este problema, deberá deshabilitar la carga diferida.

## <a name="multiple-result-sets-with-configured-in-edmx"></a>Varios conjuntos de resultados con configurado en EDMX

>[!NOTE]
> Debe tener como destino .NET Framework 4.5 para que pueda configurar varios conjuntos de resultados en EDMX. Si tiene como destino .NET 4.0, puede usar el método basado en código que se muestra en la sección anterior.

Si usa el Diseñador de EF, también puede modificar el modelo para que sepa sobre los distintos conjuntos de resultados que se devolverán. Algo que debe conocer antes de mano es que las herramientas no son resultados múltiples establece compatible con, por lo que deberá modificar manualmente el archivo edmx. Editar el archivo edmx que esto funcionará, pero también interrumpirá la validación del modelo en Visual Studio. Por lo que si la validación del modelo siempre obtendrá errores.

-   Para ello, deberá agregar el procedimiento almacenado al modelo como lo haría para una consulta de conjunto de resultados único.
-   Una vez que esto tiene que haga clic con el botón derecho en el modelo y seleccione **abrir con...** a continuación, **Xml**

    ![OpenAs](~/ef6/media/openas.png)

Una vez que tenga el modelo que se abrió como XML, deberá hacer lo siguiente:

-   Busque la importación de función y de tipo compleja en el modelo:

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

 

-   Quitar el tipo complejo
-   Actualizar la importación de función para que asigna a las entidades, en nuestro caso que tendrá un aspecto similar al siguiente:

``` xml
    <FunctionImport Name="GetAllBlogsAndPosts">
      <ReturnType EntitySet="Blogs" Type="Collection(BlogModel.Blog)" />
      <ReturnType EntitySet="Posts" Type="Collection(BlogModel.Post)" />
    </FunctionImport>
```

Esto indica que el modelo que el procedimiento almacenado devolverá dos colecciones, una de las entradas de blog y una de las entradas de la publicación.

-   Busque el elemento de asignación de función:

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

-   Reemplace la asignación de resultado por uno para cada entidad que se devuelve como el siguiente:

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

También es posible asignar los conjuntos de resultados a los tipos complejos, como el que se crean de forma predeterminada. Para ello cree un nuevo tipo complejo, en lugar de quitarlos y usar los tipos complejos en cualquier lugar que hubiera usado los nombres de entidad en los ejemplos anteriores.

Una vez que se han cambiado estas asignaciones, a continuación, puede guardar el modelo y ejecute el siguiente código para usar el procedimiento almacenado:

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
> Si edita manualmente el archivo edmx para el modelo se sobrescribirán si alguna vez se regenera el modelo de la base de datos.

## <a name="summary"></a>Resumen

Aquí hemos mostrado dos métodos diferentes de obtener acceso a varios resultados se establece mediante Entity Framework. Ambos son igualmente válidos según la situación y las preferencias y debe elegir lo que parece más adecuado para sus circunstancias. Está previsto que la compatibilidad con resultados múltiples conjuntos de serán mejorado en futuras versiones de Entity Framework y que realizar los pasos de este documento ya no será necesario.
