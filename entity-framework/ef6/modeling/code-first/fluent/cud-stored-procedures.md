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
# <a name="code-first-insert-update-and-delete-stored-procedures"></a><span data-ttu-id="82bf0-102">Primera inserción de código, actualizar y eliminar procedimientos almacenados</span><span class="sxs-lookup"><span data-stu-id="82bf0-102">Code First Insert, Update, and Delete Stored Procedures</span></span>
> [!NOTE]
> <span data-ttu-id="82bf0-103">**Solo EF6 y versiones posteriores**: las características, las API, etc. que se tratan en esta página se han incluido a partir de Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="82bf0-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="82bf0-104">Si usa una versión anterior, no se aplica parte o la totalidad de la información.</span><span class="sxs-lookup"><span data-stu-id="82bf0-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="82bf0-105">De forma predeterminada, Code First configurará todas las entidades para realizar la inserción, actualización y eliminación de comandos mediante acceso directo a tablas.</span><span class="sxs-lookup"><span data-stu-id="82bf0-105">By default, Code First will configure all entities to perform insert, update and delete commands using direct table access.</span></span> <span data-ttu-id="82bf0-106">A partir de EF6, puede configurar el modelo de Code First para utilizar procedimientos almacenados para algunas o todas las entidades del modelo.</span><span class="sxs-lookup"><span data-stu-id="82bf0-106">Starting in EF6 you can configure your Code First model to use stored procedures for some or all entities in your model.</span></span>  

## <a name="basic-entity-mapping"></a><span data-ttu-id="82bf0-107">Asignación de entidad básica</span><span class="sxs-lookup"><span data-stu-id="82bf0-107">Basic Entity Mapping</span></span>  

<span data-ttu-id="82bf0-108">Puede optar por usar procedimientos almacenados para insertar, actualizar y eliminar mediante la API Fluent.</span><span class="sxs-lookup"><span data-stu-id="82bf0-108">You can opt into using stored procedures for insert, update and delete using the Fluent API.</span></span>  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures();
```  

<span data-ttu-id="82bf0-109">Esto hará que Code First usar algunas convenciones para crear la forma esperada de los procedimientos almacenados en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="82bf0-109">Doing this will cause Code First to use some conventions to build the expected shape of the stored procedures in the database.</span></span>  

- <span data-ttu-id="82bf0-110">Tres procedimientos denominado  **\<type_name\>_Insertar**,  **\<type_name\>_actualizar** y  **\<tipo_ nombre\>_Eliminar** (por ejemplo, Blog_Insert, Blog_Update y Blog_Delete).</span><span class="sxs-lookup"><span data-stu-id="82bf0-110">Three stored procedures named **\<type_name\>_Insert**, **\<type_name\>_Update** and **\<type_name\>_Delete** (for example, Blog_Insert, Blog_Update and Blog_Delete).</span></span>  
- <span data-ttu-id="82bf0-111">Los nombres de parámetro corresponden a los nombres de propiedad.</span><span class="sxs-lookup"><span data-stu-id="82bf0-111">Parameter names correspond to the property names.</span></span>  
  > [!NOTE]
  > <span data-ttu-id="82bf0-112">Si usa HasColumnName() o el atributo de columna para cambiar el nombre de la columna para una propiedad determinada, a continuación, este nombre se usa para los parámetros en lugar del nombre de propiedad.</span><span class="sxs-lookup"><span data-stu-id="82bf0-112">If you use HasColumnName() or the Column attribute to rename the column for a given property then this name is used for parameters instead of the property name.</span></span>  
- <span data-ttu-id="82bf0-113">**El procedimiento almacenado insert** tendrá un parámetro para cada propiedad, excepto aquellos marcados como almacén generado (identidad o calculadas).</span><span class="sxs-lookup"><span data-stu-id="82bf0-113">**The insert stored procedure** will have a parameter for every property, except for those marked as store generated (identity or computed).</span></span> <span data-ttu-id="82bf0-114">El procedimiento almacenado debe devolver un conjunto de resultados con una columna para cada propiedad del almacén generado.</span><span class="sxs-lookup"><span data-stu-id="82bf0-114">The stored procedure should return a result set with a column for each store generated property.</span></span>  
- <span data-ttu-id="82bf0-115">**La actualización de procedimiento almacenado** tendrá un parámetro para cada propiedad, excepto aquellos marcados con un modelo generado de almacén de 'Computed'.</span><span class="sxs-lookup"><span data-stu-id="82bf0-115">**The update stored procedure** will have a parameter for every property, except for those marked with a store generated pattern of 'Computed'.</span></span> <span data-ttu-id="82bf0-116">Algunos tokens de simultaneidad que requieren un parámetro del valor original, consulte el *Tokens de simultaneidad* sección para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="82bf0-116">Some concurrency tokens require a parameter for the original value, see the *Concurrency Tokens* section below for details.</span></span> <span data-ttu-id="82bf0-117">El procedimiento almacenado debe devolver un conjunto de resultados con una columna para cada propiedad calculada.</span><span class="sxs-lookup"><span data-stu-id="82bf0-117">The stored procedure should return a result set with a column for each computed property.</span></span>  
- <span data-ttu-id="82bf0-118">**La eliminación de procedimiento almacenado** debe tener un parámetro para el valor de clave de la entidad (o varios parámetros, si la entidad tiene una clave compuesta).</span><span class="sxs-lookup"><span data-stu-id="82bf0-118">**The delete stored procedure** should have a parameter for the key value of the entity (or multiple parameters if the entity has a composite key).</span></span> <span data-ttu-id="82bf0-119">Además, el procedimiento de eliminación también debe tener parámetros para cualquier clave externa de la asociación independiente en la tabla de destino (relaciones que no tienen propiedades de clave externa correspondientes declaradas en la entidad).</span><span class="sxs-lookup"><span data-stu-id="82bf0-119">Additionally, the delete procedure should also have parameters for any independent association foreign keys on the target table (relationships that do not have corresponding foreign key properties declared in the entity).</span></span> <span data-ttu-id="82bf0-120">Algunos tokens de simultaneidad que requieren un parámetro del valor original, consulte el *Tokens de simultaneidad* sección para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="82bf0-120">Some concurrency tokens require a parameter for the original value, see the *Concurrency Tokens* section below for details.</span></span>  

<span data-ttu-id="82bf0-121">Uso de la clase siguiente como ejemplo:</span><span class="sxs-lookup"><span data-stu-id="82bf0-121">Using the following class as an example:</span></span>  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
}
```  

<span data-ttu-id="82bf0-122">El valor predeterminado serían procedimientos almacenados:</span><span class="sxs-lookup"><span data-stu-id="82bf0-122">The default stored procedures would be:</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="82bf0-123">Reemplazar los valores predeterminados</span><span class="sxs-lookup"><span data-stu-id="82bf0-123">Overriding the Defaults</span></span>  

<span data-ttu-id="82bf0-124">Puede invalidar la totalidad o parte de lo que se ha configurado de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="82bf0-124">You can override part or all of what was configured by default.</span></span>  

<span data-ttu-id="82bf0-125">Puede cambiar el nombre de uno o más procedimientos almacenados.</span><span class="sxs-lookup"><span data-stu-id="82bf0-125">You can change the name of one or more stored procedures.</span></span> <span data-ttu-id="82bf0-126">Este ejemplo cambia el nombre del procedimiento almacenado update solo.</span><span class="sxs-lookup"><span data-stu-id="82bf0-126">This example renames the update stored procedure only.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")));
```  

<span data-ttu-id="82bf0-127">Este ejemplo cambia el nombre de los tres procedimientos almacenados.</span><span class="sxs-lookup"><span data-stu-id="82bf0-127">This example renames all three stored procedures.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog"))  
     .Delete(d => d.HasName("delete_blog"))  
     .Insert(i => i.HasName("insert_blog")));
```  

<span data-ttu-id="82bf0-128">En estos ejemplos están encadenadas unas las llamadas, pero también puede usar la sintaxis de bloque de expresión lambda.</span><span class="sxs-lookup"><span data-stu-id="82bf0-128">In these examples the calls are chained together, but you can also use lambda block syntax.</span></span>  

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

<span data-ttu-id="82bf0-129">Este ejemplo cambia el nombre del parámetro para la propiedad BlogId en el procedimiento almacenado de actualización.</span><span class="sxs-lookup"><span data-stu-id="82bf0-129">This example renames the parameter for the BlogId property on the update stored procedure.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.BlogId, "blog_id")));
```  

<span data-ttu-id="82bf0-130">Estas llamadas están encadenadas y que admite composición.</span><span class="sxs-lookup"><span data-stu-id="82bf0-130">These calls are all chainable and composable.</span></span> <span data-ttu-id="82bf0-131">Este es un ejemplo que cambia el nombre de los tres procedimientos almacenados y sus parámetros.</span><span class="sxs-lookup"><span data-stu-id="82bf0-131">Here is an example that renames all three stored procedures and their parameters.</span></span>  

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

<span data-ttu-id="82bf0-132">También puede cambiar el nombre de las columnas del conjunto de resultados que contiene los valores de la base de datos generada.</span><span class="sxs-lookup"><span data-stu-id="82bf0-132">You can also change the name of the columns in the result set that contains database generated values.</span></span>  

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

## <a name="relationships-without-a-foreign-key-in-the-class-independent-associations"></a><span data-ttu-id="82bf0-133">Relaciones sin una clave externa de la clase (asociaciones independientes)</span><span class="sxs-lookup"><span data-stu-id="82bf0-133">Relationships Without a Foreign Key in the Class (Independent Associations)</span></span>  

<span data-ttu-id="82bf0-134">Cuando una propiedad de clave externa se incluye en la definición de clase, se puede cambiar el nombre del parámetro correspondiente en la misma manera que cualquier otra propiedad.</span><span class="sxs-lookup"><span data-stu-id="82bf0-134">When a foreign key property is included in the class definition, the corresponding parameter can be renamed in the same way as any other property.</span></span> <span data-ttu-id="82bf0-135">Cuando existe una relación sin una propiedad de clave externa en la clase, el nombre de parámetro predeterminado es  **\<navigation_property_name\>_\<primary_key_name\>**.</span><span class="sxs-lookup"><span data-stu-id="82bf0-135">When a relationship exists without a foreign key property in the class, the default parameter name is **\<navigation_property_name\>_\<primary_key_name\>**.</span></span>  

<span data-ttu-id="82bf0-136">Por ejemplo, las siguientes definiciones de clase dará como resultado un parámetro Blog_BlogId que se espera en los procedimientos almacenados para insertar y actualizar entradas.</span><span class="sxs-lookup"><span data-stu-id="82bf0-136">For example, the following class definitions would result in a Blog_BlogId parameter being expected in the stored procedures to insert and update Posts.</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="82bf0-137">Reemplazar los valores predeterminados</span><span class="sxs-lookup"><span data-stu-id="82bf0-137">Overriding the Defaults</span></span>  

<span data-ttu-id="82bf0-138">Puede cambiar los parámetros para las claves externas que no están incluidos en la clase proporcionando la ruta de acceso a la propiedad de clave principal para el método de parámetro.</span><span class="sxs-lookup"><span data-stu-id="82bf0-138">You can change parameters for foreign keys that are not included in the class by supplying the path to the primary key property to the Parameter method.</span></span>  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Parameter(p => p.Blog.BlogId, "blog_id")));
```  

<span data-ttu-id="82bf0-139">Si no tiene una propiedad de navegación en la entidad dependiente (como)</span><span class="sxs-lookup"><span data-stu-id="82bf0-139">If you don’t have a navigation property on the dependent entity (i.e</span></span> <span data-ttu-id="82bf0-140">No hay ninguna propiedad Post.Blog), a continuación, puede usar el método de asociación para identificar el otro extremo de la relación y, a continuación, configure los parámetros que corresponden a cada una de las propiedades de clave.</span><span class="sxs-lookup"><span data-stu-id="82bf0-140">no Post.Blog property) then you can use the Association method to identify the other end of the relationship and then configure the parameters that correspond to each of the key property(s).</span></span>  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Navigation<Blog>(  
      b => b.Posts,  
      c => c.Parameter(b => b.BlogId, "blog_id"))));
```  

## <a name="concurrency-tokens"></a><span data-ttu-id="82bf0-141">Tokens de simultaneidad</span><span class="sxs-lookup"><span data-stu-id="82bf0-141">Concurrency Tokens</span></span>  

<span data-ttu-id="82bf0-142">Update y delete que se almacenan los procedimientos también tendrá que tratar con la simultaneidad:</span><span class="sxs-lookup"><span data-stu-id="82bf0-142">Update and delete stored procedures may also need to deal with concurrency:</span></span>  

- <span data-ttu-id="82bf0-143">Si la entidad contiene los tokens de simultaneidad, el procedimiento almacenado puede tener opcionalmente un parámetro de salida que devuelve el número de filas actualizados o eliminados (filas afectadas).</span><span class="sxs-lookup"><span data-stu-id="82bf0-143">If the entity contains concurrency tokens, the stored procedure can optionally have an output parameter that returns the number of rows updated/deleted (rows affected).</span></span> <span data-ttu-id="82bf0-144">Esta clase de parámetro debe configurarse mediante el método RowsAffectedParameter.</span><span class="sxs-lookup"><span data-stu-id="82bf0-144">Such a parameter must be configured using the RowsAffectedParameter method.</span></span>  
<span data-ttu-id="82bf0-145">De forma predeterminada EF usa el valor devuelto de ExecuteNonQuery para determinar cuántas filas afectadas.</span><span class="sxs-lookup"><span data-stu-id="82bf0-145">By default EF uses the return value from ExecuteNonQuery to determine how many rows were affected.</span></span> <span data-ttu-id="82bf0-146">Especificar un parámetro de salida de filas afectadas es útil si realiza cualquier lógica en el procedimiento almacenado que daría como resultado el valor devuelto de ExecuteNonQuery es incorrecta (desde la perspectiva de EF) al final de la ejecución.</span><span class="sxs-lookup"><span data-stu-id="82bf0-146">Specifying a rows affected output parameter is useful if you perform any logic in your sproc that would result in the return value of ExecuteNonQuery being incorrect (from EF's perspective) at the end of execution.</span></span>  
- <span data-ttu-id="82bf0-147">Cada simultaneidad token existe será un parámetro denominado  **\<property_name\>_Original** (por ejemplo, Timestamp_Original).</span><span class="sxs-lookup"><span data-stu-id="82bf0-147">For each concurrency token there will be a parameter named **\<property_name\>_Original** (for example, Timestamp_Original ).</span></span> <span data-ttu-id="82bf0-148">Este objeto se pasa el valor original de esta propiedad, el valor cuando se consultan desde la base de datos.</span><span class="sxs-lookup"><span data-stu-id="82bf0-148">This will be passed the original value of this property – the value when queried from the database.</span></span>  
    - <span data-ttu-id="82bf0-149">Los tokens de simultaneidad que se calculan de la base de datos: por ejemplo, las marcas de tiempo: tendrá sólo un parámetro de valor original.</span><span class="sxs-lookup"><span data-stu-id="82bf0-149">Concurrency tokens that are computed by the database – such as timestamps – will only have an original value parameter.</span></span>  
    - <span data-ttu-id="82bf0-150">Propiedades calculadas no que se establecen como tokens de simultaneidad también tendrá un parámetro para el nuevo valor en el procedimiento de actualización.</span><span class="sxs-lookup"><span data-stu-id="82bf0-150">Non-computed properties that are set as concurrency tokens will also have a parameter for the new value in the update procedure.</span></span> <span data-ttu-id="82bf0-151">Esto usa las convenciones de nomenclatura descritas para los nuevos valores.</span><span class="sxs-lookup"><span data-stu-id="82bf0-151">This uses the naming conventions already discussed for new values.</span></span> <span data-ttu-id="82bf0-152">Un ejemplo de un token de este tipo sería utilizar URL de un Blog como un token de simultaneidad, el nuevo valor es necesario porque esto se puede actualizar a un nuevo valor por el código (a diferencia de un token de marca de tiempo que solo se actualiza la base de datos).</span><span class="sxs-lookup"><span data-stu-id="82bf0-152">An example of such a token would be using a Blog's URL as a concurrency token, the new value is required because this can be updated to a new value by your code (unlike a Timestamp token which is only updated by the database).</span></span>  

<span data-ttu-id="82bf0-153">Este es un ejemplo de clase y actualizar el procedimiento almacenado con un token de simultaneidad de marca de tiempo.</span><span class="sxs-lookup"><span data-stu-id="82bf0-153">This is an example class and update stored procedure with a timestamp concurrency token.</span></span>  

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

<span data-ttu-id="82bf0-154">Este es un ejemplo de clase y actualizar el procedimiento almacenado con el token de simultaneidad no calculadas.</span><span class="sxs-lookup"><span data-stu-id="82bf0-154">Here is an example class and update stored procedure with non-computed concurrency token.</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="82bf0-155">Reemplazar los valores predeterminados</span><span class="sxs-lookup"><span data-stu-id="82bf0-155">Overriding the Defaults</span></span>  

<span data-ttu-id="82bf0-156">Si lo desea, puede introducir un parámetro de filas afectadas.</span><span class="sxs-lookup"><span data-stu-id="82bf0-156">You can optionally introduce a rows affected parameter.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.RowsAffectedParameter("rows_affected")));
```  

<span data-ttu-id="82bf0-157">Para los tokens de simultaneidad de base de datos calculada: donde se pasa sólo el valor original: solo puede usar el mecanismo de cambio de nombre de parámetro estándar para cambiar el nombre del parámetro del valor original.</span><span class="sxs-lookup"><span data-stu-id="82bf0-157">For database computed concurrency tokens – where only the original value is passed – you can just use the standard parameter renaming mechanism to rename the parameter for the original value.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.Timestamp, "blog_timestamp")));
```  

<span data-ttu-id="82bf0-158">Para los tokens de simultaneidad no calculada, donde tanto el valor original y nuevo se pasa: puede usar una sobrecarga del parámetro que le permite proporcionar un nombre para cada parámetro.</span><span class="sxs-lookup"><span data-stu-id="82bf0-158">For non-computed concurrency tokens – where both the original and new value are passed – you can use an overload of Parameter that allows you to supply a name for each parameter.</span></span>  

``` csharp
modelBuilder
 .Entity<Blog>()
 .MapToStoredProcedures(s => s.Update(u => u.Parameter(b => b.Url, "blog_url", "blog_original_url")));
```  

## <a name="many-to-many-relationships"></a><span data-ttu-id="82bf0-159">Muchos a muchos relaciones</span><span class="sxs-lookup"><span data-stu-id="82bf0-159">Many to Many Relationships</span></span>  

<span data-ttu-id="82bf0-160">Vamos a usar las clases siguientes como ejemplo en esta sección.</span><span class="sxs-lookup"><span data-stu-id="82bf0-160">We’ll use the following classes as an example in this section.</span></span>  

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

<span data-ttu-id="82bf0-161">Varios a muchas relaciones pueden asignarse a procedimientos almacenados con la siguiente sintaxis.</span><span class="sxs-lookup"><span data-stu-id="82bf0-161">Many to many relationships can be mapped to stored procedures with the following syntax.</span></span>  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures();
```  

<span data-ttu-id="82bf0-162">Si no se proporciona ninguna otra configuración, a continuación, la forma de procedimiento almacenado siguiente se usa de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="82bf0-162">If no other configuration is supplied then the following stored procedure shape is used by default.</span></span>  

- <span data-ttu-id="82bf0-163">Dos procedimientos denominados almacenados  **\<type_one\>\<type_two\>_Insertar** y  **\<type_one\>\<type_two \>_Eliminar** (por ejemplo, PostTag_Insert y PostTag_Delete).</span><span class="sxs-lookup"><span data-stu-id="82bf0-163">Two stored procedures named **\<type_one\>\<type_two\>_Insert** and **\<type_one\>\<type_two\>_Delete** (for example, PostTag_Insert and PostTag_Delete).</span></span>  
- <span data-ttu-id="82bf0-164">Los parámetros serán los valores de clave para cada tipo.</span><span class="sxs-lookup"><span data-stu-id="82bf0-164">The parameters will be the key value(s) for each type.</span></span> <span data-ttu-id="82bf0-165">El nombre de cada parámetro que se va a **\<type_name\>_\<property_name\>** (por ejemplo, Post_PostId y Tag_TagId).</span><span class="sxs-lookup"><span data-stu-id="82bf0-165">The name of each parameter being **\<type_name\>_\<property_name\>** (for example, Post_PostId and Tag_TagId).</span></span>

<span data-ttu-id="82bf0-166">Éstos son ejemplos de insertar y actualizar los procedimientos almacenados.</span><span class="sxs-lookup"><span data-stu-id="82bf0-166">Here are example insert and update stored procedures.</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="82bf0-167">Reemplazar los valores predeterminados</span><span class="sxs-lookup"><span data-stu-id="82bf0-167">Overriding the Defaults</span></span>  

<span data-ttu-id="82bf0-168">El procedimiento y nombres de parámetro se pueden configurar de forma similar a los procedimientos almacenado de entidad.</span><span class="sxs-lookup"><span data-stu-id="82bf0-168">The procedure and parameter names can be configured in a similar way to entity stored procedures.</span></span>  

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
