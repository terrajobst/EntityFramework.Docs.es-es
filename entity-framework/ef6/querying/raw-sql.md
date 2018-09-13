---
title: Consultas SQL sin formato - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9e1ee76e-2499-408c-81e8-9b6c5d1945a0
ms.openlocfilehash: 6b00648939ccedffeed09b4e1d6e8d70fa262a36
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490589"
---
# <a name="raw-sql-queries"></a>Consultas SQL sin formato
Entity Framework le permite realizar consultas con LINQ con las clases de entidad. Sin embargo, puede haber ocasiones en los que desea ejecutar consultas con SQL sin procesar directamente en la base de datos. Esto incluye una llamada a procedimientos almacenados, que pueden ser útiles para los modelos de Code First que actualmente no admiten la asignación a los procedimientos almacenados. Las técnicas que se muestran en este tema se aplican igualmente a los modelos creados con Code First y EF Designer.  

## <a name="writing-sql-queries-for-entities"></a>Escribir consultas SQL para entidades  

El método SqlQuery en DbSet permite que una consulta SQL sin procesar para escribirse en la que va a devolver instancias de entidad. Los objetos devueltos se realizará un seguimiento por el contexto tal como lo serían si ha devuelto una consulta LINQ. Por ejemplo:  

``` csharp  
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("SELECT * FROM dbo.Blogs").ToList();
}
```  

Tenga en cuenta que, al igual que para las consultas LINQ, la consulta no se ejecuta hasta que se enumeran los resultados, en el ejemplo anterior se realiza con la llamada a ToList.  

Debe tener cuidado cuando se escriben consultas SQL sin procesar por dos motivos. En primer lugar, se debe escribir la consulta para asegurarse de que solo devuelve las entidades que son realmente del tipo solicitado. Por ejemplo, cuando se usan características como la herencia es fácil escribir una consulta que va a crear las entidades que son del tipo CLR incorrecto.  

En segundo lugar, algunos tipos de consulta SQL sin formato exponen posibles riesgos de seguridad, especialmente en torno a los ataques de inyección SQL. Asegúrese de utilizar parámetros en la consulta de la manera correcta para protegerse contra dichos ataques.  

### <a name="loading-entities-from-stored-procedures"></a>Cargar entidades desde procedimientos almacenados  

Puede usar DbSet.SqlQuery para cargar las entidades de los resultados de un procedimiento almacenado. Por ejemplo, el código siguiente llama a dbo. Procedimiento GetBlogs en la base de datos:  

``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("dbo.GetBlogs").ToList();
}
```  

También puede pasar parámetros a un procedimiento almacenado mediante la sintaxis siguiente:  

``` csharp
using (var context = new BloggingContext())
{
    var blogId = 1;

    var blogs = context.Blogs.SqlQuery("dbo.GetBlogById @p0", blogId).Single();
}
```  

## <a name="writing-sql-queries-for-non-entity-types"></a>Escribir consultas SQL para tipos sin entidad  

Una consulta SQL que devuelve instancias de cualquier tipo, incluidos los tipos primitivos, puede crearse mediante el método SqlQuery de la clase de base de datos. Por ejemplo:  

``` csharp
using (var context = new BloggingContext())
{
    var blogNames = context.Database.SqlQuery<string>(
                       "SELECT Name FROM dbo.Blogs").ToList();
}
```  

Los resultados devueltos por la consulta SQL en la base de datos nunca realizará el seguimiento por el contexto incluso si los objetos son instancias de un tipo de entidad.  

## <a name="sending-raw-commands-to-the-database"></a>Envío de comandos sin procesar a la base de datos  

Comandos de la consulta no se pueden enviar a la base de datos mediante el método ExecuteSqlCommand en la base de datos. Por ejemplo:  

``` csharp
using (var context = new BloggingContext())
{
    context.Database.ExecuteSqlCommand(
        "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
}
```  

Tenga en cuenta que los cambios realizados en los datos en la base de datos mediante ExecuteSqlCommand son opacos para el contexto hasta que se cargan o se vuelve a cargar desde la base de datos de entidades.  

### <a name="output-parameters"></a>Parámetros de salida  

Si se usan parámetros de salida, sus valores no estará disponibles hasta que se han leído completamente los resultados. Esto es debido al comportamiento subyacente de DbDataReader, vea [recuperar datos mediante DataReader](http://go.microsoft.com/fwlink/?LinkID=398589) para obtener más detalles.  
