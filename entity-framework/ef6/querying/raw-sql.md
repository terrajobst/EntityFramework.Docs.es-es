---
title: 'Consultas SQL sin procesar: EF6'
author: divega
ms.date: 10/23/2016
ms.assetid: 9e1ee76e-2499-408c-81e8-9b6c5d1945a0
ms.openlocfilehash: 168aee67186535bf2a50705e86bfc5a88147e369
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414474"
---
# <a name="raw-sql-queries"></a>Consultas SQL sin formato
Entity Framework permite realizar consultas con LINQ con las clases de entidad. Sin embargo, puede haber ocasiones en las que desee ejecutar consultas utilizando SQL sin formato directamente en la base de datos. Esto incluye llamar a procedimientos almacenados, que pueden ser útiles para Code First modelos que actualmente no admiten la asignación a procedimientos almacenados. Las técnicas que se muestran en este tema se aplican igualmente a los modelos creados con Code First y EF Designer.  

## <a name="writing-sql-queries-for-entities"></a>Escribir consultas SQL para entidades  

El método SqlQuery en DbSet permite escribir una consulta SQL sin formato que devolverá las instancias de la entidad. El contexto realizará un seguimiento de los objetos devueltos tal como lo haría si se devolvieran mediante una consulta LINQ. Por ejemplo:  

``` csharp  
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("SELECT * FROM dbo.Blogs").ToList();
}
```  

Tenga en cuenta que, al igual que para las consultas LINQ, la consulta no se ejecuta hasta que se enumeran los resultados; en el ejemplo anterior, esto se realiza con la llamada a ToList.  

Se debe tener cuidado cuando las consultas SQL sin procesar se escriben por dos motivos. En primer lugar, se debe escribir la consulta para asegurarse de que solo devuelve entidades que son realmente del tipo solicitado. Por ejemplo, al usar características como la herencia, es fácil escribir una consulta que creará entidades que son del tipo CLR incorrecto.  

En segundo lugar, algunos tipos de consultas SQL sin procesar exponen posibles riesgos de seguridad, especialmente en torno a ataques por inyección de SQL. Asegúrese de usar los parámetros de la consulta de la manera correcta para protegerse frente a estos ataques.  

### <a name="loading-entities-from-stored-procedures"></a>Cargar entidades desde procedimientos almacenados  

Puede usar DbSet. SqlQuery para cargar entidades de los resultados de un procedimiento almacenado. Por ejemplo, el código siguiente llama a DBO. Procedimiento GetBlogs en la base de datos:  

``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("dbo.GetBlogs").ToList();
}
```  

También puede pasar parámetros a un procedimiento almacenado con la sintaxis siguiente:  

``` csharp
using (var context = new BloggingContext())
{
    var blogId = 1;

    var blogs = context.Blogs.SqlQuery("dbo.GetBlogById @p0", blogId).Single();
}
```  

## <a name="writing-sql-queries-for-non-entity-types"></a>Escribir consultas SQL para tipos que no son de entidad  

Una consulta SQL que devuelve instancias de cualquier tipo, incluidos los tipos primitivos, se puede crear con el método SqlQuery en la clase de base de datos. Por ejemplo:  

``` csharp
using (var context = new BloggingContext())
{
    var blogNames = context.Database.SqlQuery<string>(
                       "SELECT Name FROM dbo.Blogs").ToList();
}
```  

Nunca se realizará el seguimiento de los resultados devueltos de SqlQuery en la base de datos en el contexto, incluso si los objetos son instancias de un tipo de entidad.  

## <a name="sending-raw-commands-to-the-database"></a>Enviar comandos sin formato a la base de datos  

Los comandos que no son de consulta se pueden enviar a la base de datos mediante el método ExecuteSqlCommand en la base de datos. Por ejemplo:  

``` csharp
using (var context = new BloggingContext())
{
    context.Database.ExecuteSqlCommand(
        "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
}
```  

Tenga en cuenta que los cambios realizados en los datos de la base de datos mediante ExecuteSqlCommand son opacos en el contexto hasta que las entidades se cargan o se vuelven a cargar desde la base de datos.  

### <a name="output-parameters"></a>Parámetros de salida  

Si se utilizan parámetros de salida, sus valores no estarán disponibles hasta que los resultados se hayan leído por completo. Esto se debe al comportamiento subyacente de DbDataReader, consulte [recuperación de datos mediante un DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) para obtener más detalles.  
