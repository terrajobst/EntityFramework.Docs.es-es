---
title: Consultas SQL sin formato - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 9e1ee76e-2499-408c-81e8-9b6c5d1945a0
ms.openlocfilehash: 99893ca1c634ce6f2e4cf9dcb70b1a1e43532c60
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995739"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="79074-102">Consultas SQL sin formato</span><span class="sxs-lookup"><span data-stu-id="79074-102">Raw SQL Queries</span></span>
<span data-ttu-id="79074-103">Entity Framework le permite realizar consultas con LINQ con las clases de entidad.</span><span class="sxs-lookup"><span data-stu-id="79074-103">Entity Framework allows you to query using LINQ with your entity classes.</span></span> <span data-ttu-id="79074-104">Sin embargo, puede haber ocasiones en los que desea ejecutar consultas con SQL sin procesar directamente en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="79074-104">However, there may be times that you want to run queries using raw SQL directly against the database.</span></span> <span data-ttu-id="79074-105">Esto incluye una llamada a procedimientos almacenados, que pueden ser útiles para los modelos de Code First que actualmente no admiten la asignación a los procedimientos almacenados.</span><span class="sxs-lookup"><span data-stu-id="79074-105">This includes calling stored procedures, which can be helpful for Code First models that currently do not support mapping to stored procedures.</span></span> <span data-ttu-id="79074-106">Las técnicas que se muestran en este tema se aplican igualmente a los modelos creados con Code First y EF Designer.</span><span class="sxs-lookup"><span data-stu-id="79074-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="writing-sql-queries-for-entities"></a><span data-ttu-id="79074-107">Escribir consultas SQL para entidades</span><span class="sxs-lookup"><span data-stu-id="79074-107">Writing SQL queries for entities</span></span>  

<span data-ttu-id="79074-108">El método SqlQuery en DbSet permite que una consulta SQL sin procesar para escribirse en la que va a devolver instancias de entidad.</span><span class="sxs-lookup"><span data-stu-id="79074-108">The SqlQuery method on DbSet allows a raw SQL query to be written that will return entity instances.</span></span> <span data-ttu-id="79074-109">Los objetos devueltos se realizará un seguimiento por el contexto tal como lo serían si ha devuelto una consulta LINQ.</span><span class="sxs-lookup"><span data-stu-id="79074-109">The returned objects will be tracked by the context just as they would be if they were returned by a LINQ query.</span></span> <span data-ttu-id="79074-110">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="79074-110">For example:</span></span>  

``` csharp  
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("SELECT * FROM dbo.Blogs").ToList();
}
```  

<span data-ttu-id="79074-111">Tenga en cuenta que, al igual que para las consultas LINQ, la consulta no se ejecuta hasta que se enumeran los resultados, en el ejemplo anterior se realiza con la llamada a ToList.</span><span class="sxs-lookup"><span data-stu-id="79074-111">Note that, just as for LINQ queries, the query is not executed until the results are enumerated—in the example above this is done with the call to ToList.</span></span>  

<span data-ttu-id="79074-112">Debe tener cuidado cuando se escriben consultas SQL sin procesar por dos motivos.</span><span class="sxs-lookup"><span data-stu-id="79074-112">Care should be taken whenever raw SQL queries are written for two reasons.</span></span> <span data-ttu-id="79074-113">En primer lugar, se debe escribir la consulta para asegurarse de que solo devuelve las entidades que son realmente del tipo solicitado.</span><span class="sxs-lookup"><span data-stu-id="79074-113">First, the query should be written to ensure that it only returns entities that are really of the requested type.</span></span> <span data-ttu-id="79074-114">Por ejemplo, cuando se usan características como la herencia es fácil escribir una consulta que va a crear las entidades que son del tipo CLR incorrecto.</span><span class="sxs-lookup"><span data-stu-id="79074-114">For example, when using features such as inheritance it is easy to write a query that will create entities that are of the wrong CLR type.</span></span>  

<span data-ttu-id="79074-115">En segundo lugar, algunos tipos de consulta SQL sin formato exponen posibles riesgos de seguridad, especialmente en torno a los ataques de inyección SQL.</span><span class="sxs-lookup"><span data-stu-id="79074-115">Second, some types of raw SQL query expose potential security risks, especially around SQL injection attacks.</span></span> <span data-ttu-id="79074-116">Asegúrese de utilizar parámetros en la consulta de la manera correcta para protegerse contra dichos ataques.</span><span class="sxs-lookup"><span data-stu-id="79074-116">Make sure that you use parameters in your query in the correct way to guard against such attacks.</span></span>  

### <a name="loading-entities-from-stored-procedures"></a><span data-ttu-id="79074-117">Cargar entidades desde procedimientos almacenados</span><span class="sxs-lookup"><span data-stu-id="79074-117">Loading entities from stored procedures</span></span>  

<span data-ttu-id="79074-118">Puede usar DbSet.SqlQuery para cargar las entidades de los resultados de un procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="79074-118">You can use DbSet.SqlQuery to load entities from the results of a stored procedure.</span></span> <span data-ttu-id="79074-119">Por ejemplo, el código siguiente llama a dbo. Procedimiento GetBlogs en la base de datos:</span><span class="sxs-lookup"><span data-stu-id="79074-119">For example, the following code calls the dbo.GetBlogs procedure in the database:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("dbo.GetBlogs").ToList();
}
```  

<span data-ttu-id="79074-120">También puede pasar parámetros a un procedimiento almacenado mediante la sintaxis siguiente:</span><span class="sxs-lookup"><span data-stu-id="79074-120">You can also pass parameters to a stored procedure using the following syntax:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogId = 1;

    var blogs = context.Blogs.SqlQuery("dbo.GetBlogById @p0", blogId).Single();
}
```  

## <a name="writing-sql-queries-for-non-entity-types"></a><span data-ttu-id="79074-121">Escribir consultas SQL para tipos sin entidad</span><span class="sxs-lookup"><span data-stu-id="79074-121">Writing SQL queries for non-entity types</span></span>  

<span data-ttu-id="79074-122">Una consulta SQL que devuelve instancias de cualquier tipo, incluidos los tipos primitivos, puede crearse mediante el método SqlQuery de la clase de base de datos.</span><span class="sxs-lookup"><span data-stu-id="79074-122">A SQL query returning instances of any type, including primitive types, can be created using the SqlQuery method on the Database class.</span></span> <span data-ttu-id="79074-123">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="79074-123">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogNames = context.Database.SqlQuery<string>(
                       "SELECT Name FROM dbo.Blogs").ToList();
}
```  

<span data-ttu-id="79074-124">Los resultados devueltos por la consulta SQL en la base de datos nunca realizará el seguimiento por el contexto incluso si los objetos son instancias de un tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="79074-124">The results returned from SqlQuery on Database will never be tracked by the context even if the objects are instances of an entity type.</span></span>  

## <a name="sending-raw-commands-to-the-database"></a><span data-ttu-id="79074-125">Envío de comandos sin procesar a la base de datos</span><span class="sxs-lookup"><span data-stu-id="79074-125">Sending raw commands to the database</span></span>  

<span data-ttu-id="79074-126">Comandos de la consulta no se pueden enviar a la base de datos mediante el método ExecuteSqlCommand en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="79074-126">Non-query commands can be sent to the database using the ExecuteSqlCommand method on Database.</span></span> <span data-ttu-id="79074-127">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="79074-127">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    context.Database.ExecuteSqlCommand(
        "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
}
```  

<span data-ttu-id="79074-128">Tenga en cuenta que los cambios realizados en los datos en la base de datos mediante ExecuteSqlCommand son opacos para el contexto hasta que se cargan o se vuelve a cargar desde la base de datos de entidades.</span><span class="sxs-lookup"><span data-stu-id="79074-128">Note that any changes made to data in the database using ExecuteSqlCommand are opaque to the context until entities are loaded or reloaded from the database.</span></span>  

### <a name="output-parameters"></a><span data-ttu-id="79074-129">Parámetros de salida</span><span class="sxs-lookup"><span data-stu-id="79074-129">Output Parameters</span></span>  

<span data-ttu-id="79074-130">Si se usan parámetros de salida, sus valores no estará disponibles hasta que se han leído completamente los resultados.</span><span class="sxs-lookup"><span data-stu-id="79074-130">If output parameters are used, their values will not be available until the results have been read completely.</span></span> <span data-ttu-id="79074-131">Esto es debido al comportamiento subyacente de DbDataReader, vea [recuperar datos mediante DataReader](http://go.microsoft.com/fwlink/?LinkID=398589) para obtener más detalles.</span><span class="sxs-lookup"><span data-stu-id="79074-131">This is due to the underlying behavior of DbDataReader, see [Retrieving Data Using a DataReader](http://go.microsoft.com/fwlink/?LinkID=398589) for more details.</span></span>  
