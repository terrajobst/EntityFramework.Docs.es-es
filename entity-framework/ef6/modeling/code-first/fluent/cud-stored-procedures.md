---
title: 'Primera inserción de código, actualizar y eliminar procedimientos almacenados: EF6'
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 9a7ae7f9-4072-4843-877d-506dd7eef576
caps.latest.revision: 3
ms.openlocfilehash: 1f100ed888abd98df83c80d0de2086cfb1ba7b4f
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122746"
---
# <a name="code-first-insert-update-and-delete-stored-procedures"></a>Primera inserción de código, actualizar y eliminar procedimientos almacenados
> [!NOTE]
> **Solo EF6 y versiones posteriores**: las características, las API, etc. que se tratan en esta página se han incluido a partir de Entity Framework 6. Si usa una versión anterior, no se aplica parte o la totalidad de la información.  

De forma predeterminada, Code First configurará todas las entidades para realizar la inserción, actualización y eliminación de comandos mediante acceso directo a tablas. A partir de EF6, puede configurar el modelo de Code First para utilizar procedimientos almacenados para algunas o todas las entidades del modelo.  

## <a name="basic-entity-mapping"></a>Asignación de entidad básica  

Puede optar por usar procedimientos almacenados para insertar, actualizar y eliminar mediante la API Fluent.  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures();
```  

Esto hará que Code First usar algunas convenciones para crear la forma esperada de los procedimientos almacenados en la base de datos.  

- Tres procedimientos denominado  **\<type_name\>_Insertar**,  **\<type_name\>_actualizar** y  **\<tipo_ nombre\>_Eliminar** (por ejemplo, Blog_Insert, Blog_Update y Blog_Delete).  
- Los nombres de parámetro corresponden a los nombres de propiedad.  
  > [!NOTE]
  > Si usa HasColumnName() o el atributo de columna para cambiar el nombre de la columna para una propiedad determinada, a continuación, este nombre se usa para los parámetros en lugar del nombre de propiedad.  
- **El procedimiento almacenado insert** tendrá un parámetro para cada propiedad, excepto aquellos marcados como almacén generado (identidad o calculadas). El procedimiento almacenado debe devolver un conjunto de resultados con una columna para cada propiedad del almacén generado.  
- **La actualización de procedimiento almacenado** tendrá un parámetro para cada propiedad, excepto aquellos marcados con un modelo generado de almacén de 'Computed'. Algunos tokens de simultaneidad que requieren un parámetro del valor original, consulte el *Tokens de simultaneidad* sección para obtener más información. El procedimiento almacenado debe devolver un conjunto de resultados con una columna para cada propiedad calculada.  
- **La eliminación de procedimiento almacenado** debe tener un parámetro para el valor de clave de la entidad (o varios parámetros, si la entidad tiene una clave compuesta). Además, el procedimiento de eliminación también debe tener parámetros para cualquier clave externa de la asociación independiente en la tabla de destino (relaciones que no tienen propiedades de clave externa correspondientes declaradas en la entidad). Algunos tokens de simultaneidad que requieren un parámetro del valor original, consulte el *Tokens de simultaneidad* sección para obtener más información.  

Uso de la clase siguiente como ejemplo:  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
}
```  

El valor predeterminado serían procedimientos almacenados:  

``` SQL
CREATE PROCEDURE [dbo].[Blog_Insert]  
  @Name nvarchar(max),  
  @Url nvarchar(max)  
AS  
BEGIN
  INSERT INTO [dbo].[Blogs] ([Name], [Url])
  VALUES (@Name, @Url)

  SELECT SCOPE_IDENTITY() AS BlogId
END
CREATE PROCEDURE [dbo].[Blog_Update]  
  @BlogId int,  
  @Name nvarchar(max),  
  @Url nvarchar(max)  
AS  
  UPDATE [dbo].[Blogs]
  SET [Name] = @Name, [Url] = @Url     
  WHERE BlogId = @BlogId;
CREATE PROCEDURE [dbo].[Blog_Delete]  
  @BlogId int  
AS  
  DELETE FROM [dbo].[Blogs]
  WHERE BlogId = @BlogId
```  

### <a name="overriding-the-defaults"></a>Reemplazar los valores predeterminados  

Puede invalidar la totalidad o parte de lo que se ha configurado de forma predeterminada.  

Puede cambiar el nombre de uno o más procedimientos almacenados. Este ejemplo cambia el nombre del procedimiento almacenado update solo.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")));
```  

Este ejemplo cambia el nombre de los tres procedimientos almacenados.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog"))  
     .Delete(d => d.HasName("delete_blog"))  
     .Insert(i => i.HasName("insert_blog")));
```  

En estos ejemplos están encadenadas unas las llamadas, pero también puede usar la sintaxis de bloque de expresión lambda.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    {  
      s.Update(u => u.HasName("modify_blog"));  
      s.Delete(d => d.HasName("delete_blog"));  
      s.Insert(i => i.HasName("insert_blog"));  
    });
```  

Este ejemplo cambia el nombre del parámetro para la propiedad BlogId en el procedimiento almacenado de actualización.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.BlogId, "blog_id")));
```  

Estas llamadas están encadenadas y que admite composición. Este es un ejemplo que cambia el nombre de los tres procedimientos almacenados y sus parámetros.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")  
                   .Parameter(b => b.BlogId, "blog_id")  
                   .Parameter(b => b.Name, "blog_name")  
                   .Parameter(b => b.Url, "blog_url"))  
     .Delete(d => d.HasName("delete_blog")  
                   .Parameter(b => b.BlogId, "blog_id"))  
     .Insert(i => i.HasName("insert_blog")  
                   .Parameter(b => b.Name, "blog_name")  
                   .Parameter(b => b.Url, "blog_url")));
```  

También puede cambiar el nombre de las columnas del conjunto de resultados que contiene los valores de la base de datos generada.  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures(s =>
    s.Insert(i => i.Result(b => b.BlogId, "generated_blog_identity")));
```

``` SQL
CREATE PROCEDURE [dbo].[Blog_Insert]  
  @Name nvarchar(max),  
  @Url nvarchar(max)  
AS  
BEGIN
  INSERT INTO [dbo].[Blogs] ([Name], [Url])
  VALUES (@Name, @Url)

  SELECT SCOPE_IDENTITY() AS generated_blog_id
END
```  

## <a name="relationships-without-a-foreign-key-in-the-class-independent-associations"></a>Relaciones sin una clave externa de la clase (asociaciones independientes)  

Cuando una propiedad de clave externa se incluye en la definición de clase, se puede cambiar el nombre del parámetro correspondiente en la misma manera que cualquier otra propiedad. Cuando existe una relación sin una propiedad de clave externa en la clase, el nombre de parámetro predeterminado es  **\<navigation_property_name\>_\<primary_key_name\>**.  

Por ejemplo, las siguientes definiciones de clase dará como resultado un parámetro Blog_BlogId que se espera en los procedimientos almacenados para insertar y actualizar entradas.  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }

  public List<Post> Posts { get; set; }  
}  

public class Post  
{  
  public int PostId { get; set; }  
  public string Title { get; set; }  
  public string Content { get; set; }  

  public Blog Blog { get; set; }  
}
```  

### <a name="overriding-the-defaults"></a>Reemplazar los valores predeterminados  

Puede cambiar los parámetros para las claves externas que no están incluidos en la clase proporcionando la ruta de acceso a la propiedad de clave principal para el método de parámetro.  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Parameter(p => p.Blog.BlogId, "blog_id")));
```  

Si no tiene una propiedad de navegación en la entidad dependiente (como) No hay ninguna propiedad Post.Blog), a continuación, puede usar el método de asociación para identificar el otro extremo de la relación y, a continuación, configure los parámetros que corresponden a cada una de las propiedades de clave.  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Navigation<Blog>(  
      b => b.Posts,  
      c => c.Parameter(b => b.BlogId, "blog_id"))));
```  

## <a name="concurrency-tokens"></a>Tokens de simultaneidad  

Update y delete que se almacenan los procedimientos también tendrá que tratar con la simultaneidad:  

- Si la entidad contiene los tokens de simultaneidad, el procedimiento almacenado puede tener opcionalmente un parámetro de salida que devuelve el número de filas actualizados o eliminados (filas afectadas). Esta clase de parámetro debe configurarse mediante el método RowsAffectedParameter.  
De forma predeterminada EF usa el valor devuelto de ExecuteNonQuery para determinar cuántas filas afectadas. Especificar un parámetro de salida de filas afectadas es útil si realiza cualquier lógica en el procedimiento almacenado que daría como resultado el valor devuelto de ExecuteNonQuery es incorrecta (desde la perspectiva de EF) al final de la ejecución.  
- Cada simultaneidad token existe será un parámetro denominado  **\<property_name\>_Original** (por ejemplo, Timestamp_Original). Este objeto se pasa el valor original de esta propiedad, el valor cuando se consultan desde la base de datos.  
    - Los tokens de simultaneidad que se calculan de la base de datos: por ejemplo, las marcas de tiempo: tendrá sólo un parámetro de valor original.  
    - Propiedades calculadas no que se establecen como tokens de simultaneidad también tendrá un parámetro para el nuevo valor en el procedimiento de actualización. Esto usa las convenciones de nomenclatura descritas para los nuevos valores. Un ejemplo de un token de este tipo sería utilizar URL de un Blog como un token de simultaneidad, el nuevo valor es necesario porque esto se puede actualizar a un nuevo valor por el código (a diferencia de un token de marca de tiempo que solo se actualiza la base de datos).  

Este es un ejemplo de clase y actualizar el procedimiento almacenado con un token de simultaneidad de marca de tiempo.  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
  [Timestamp]
  public byte[] Timestamp { get; set; }
}
```

``` SQL
CREATE PROCEDURE [dbo].[Blog_Update]  
  @BlogId int,  
  @Name nvarchar(max),  
  @Url nvarchar(max),
  @Timestamp_Original rowversion  
AS  
  UPDATE [dbo].[Blogs]
  SET [Name] = @Name, [Url] = @Url     
  WHERE BlogId = @BlogId AND [Timestamp] = @Timestamp_Original
```  

Este es un ejemplo de clase y actualizar el procedimiento almacenado con el token de simultaneidad no calculadas.  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  [ConcurrencyCheck]
  public string Url { get; set; }  
}
```

``` SQL
CREATE PROCEDURE [dbo].[Blog_Update]  
  @BlogId int,  
  @Name nvarchar(max),  
  @Url nvarchar(max),
  @Url_Original nvarchar(max),
AS  
  UPDATE [dbo].[Blogs]
  SET [Name] = @Name, [Url] = @Url     
  WHERE BlogId = @BlogId AND [Url] = @Url_Original
```  

### <a name="overriding-the-defaults"></a>Reemplazar los valores predeterminados  

Si lo desea, puede introducir un parámetro de filas afectadas.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.RowsAffectedParameter("rows_affected")));
```  

Para los tokens de simultaneidad de base de datos calculada: donde se pasa sólo el valor original: solo puede usar el mecanismo de cambio de nombre de parámetro estándar para cambiar el nombre del parámetro del valor original.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.Timestamp, "blog_timestamp")));
```  

Para los tokens de simultaneidad no calculada, donde tanto el valor original y nuevo se pasa: puede usar una sobrecarga del parámetro que le permite proporcionar un nombre para cada parámetro.  

``` csharp
modelBuilder
 .Entity<Blog>()
 .MapToStoredProcedures(s => s.Update(u => u.Parameter(b => b.Url, "blog_url", "blog_original_url")));
```  

## <a name="many-to-many-relationships"></a>Muchos a muchos relaciones  

Vamos a usar las clases siguientes como ejemplo en esta sección.  

``` csharp
public class Post  
{  
  public int PostId { get; set; }  
  public string Title { get; set; }  
  public string Content { get; set; }  

  public List<Tag> Tags { get; set; }  
}  

public class Tag  
{  
  public int TagId { get; set; }  
  public string TagName { get; set; }  

  public List<Post> Posts { get; set; }  
}
```  

Varios a muchas relaciones pueden asignarse a procedimientos almacenados con la siguiente sintaxis.  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures();
```  

Si no se proporciona ninguna otra configuración, a continuación, la forma de procedimiento almacenado siguiente se usa de forma predeterminada.  

- Dos procedimientos denominados almacenados  **\<type_one\>\<type_two\>_Insertar** y  **\<type_one\>\<type_two \>_Eliminar** (por ejemplo, PostTag_Insert y PostTag_Delete).  
- Los parámetros serán los valores de clave para cada tipo. El nombre de cada parámetro que se va a **\<type_name\>_\<property_name\>** (por ejemplo, Post_PostId y Tag_TagId).

Éstos son ejemplos de insertar y actualizar los procedimientos almacenados.  

``` SQL
CREATE PROCEDURE [dbo].[PostTag_Insert]  
  @Post_PostId int,  
  @Tag_TagId int  
AS  
  INSERT INTO [dbo].[Post_Tags] (Post_PostId, Tag_TagId)   
  VALUES (@Post_PostId, @Tag_TagId)
CREATE PROCEDURE [dbo].[PostTag_Delete]  
  @Post_PostId int,  
  @Tag_TagId int  
AS  
  DELETE FROM [dbo].[Post_Tags]    
  WHERE Post_PostId = @Post_PostId AND Tag_TagId = @Tag_TagId
```  

### <a name="overriding-the-defaults"></a>Reemplazar los valores predeterminados  

El procedimiento y nombres de parámetro se pueden configurar de forma similar a los procedimientos almacenado de entidad.  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.HasName("add_post_tag")  
                   .LeftKeyParameter(p => p.PostId, "post_id")  
                   .RightKeyParameter(t => t.TagId, "tag_id"))  
     .Delete(d => d.HasName("remove_post_tag")  
                   .LeftKeyParameter(p => p.PostId, "post_id")  
                   .RightKeyParameter(t => t.TagId, "tag_id")));
```  
