---
title: Validación - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 77d6a095-c0d0-471e-80b9-8f9aea6108b2
caps.latest.revision: 3
ms.openlocfilehash: 758865255d7868337dc1d7801bd9ff77f0bb57a9
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122776"
---
# <a name="data-validation"></a>Validación de datos
> [!NOTE]
> **EF4.1 y versiones posteriores solo** -las características, las API, etc. se describe en esta página se introdujeron en Entity Framework 4.1. Si utiliza una versión anterior, algunos o todos de la información no se aplica

El contenido de esta página se ha adaptado del y el artículo originalmente escrito por Julie Lerman ([http://thedatafarm.com](http://thedatafarm.com)).

Entity Framework proporciona una gran variedad de características de validación que pueda proporcionar a través de una interfaz de usuario para la validación del lado cliente o usarse para la validación del lado servidor. Cuando se usa el código en primer lugar, puede especificar las validaciones con anotación o configuraciones de la API fluidas. Validaciones adicionales y más compleja, pueden especificarse en el código y funcionarán si el modelo de estado desde el código en primer lugar, en primer lugar el modelo o base de datos en primer lugar.

## <a name="the-model"></a>El modelo

Voy a demostrar las validaciones con un simple par de clases: Blog y Post.

``` csharp
    public class Blog
      {
          public int Id { get; set; }
          public string Title { get; set; }
          public string BloggerName { get; set; }
          public DateTime DateCreated { get; set; }
          public virtual ICollection<Post> Posts { get; set; }
          }
      }

      public class Post
      {
          public int Id { get; set; }
          public string Title { get; set; }
          public DateTime DateCreated { get; set; }
          public string Content { get; set; }
          public int BlogId { get; set; }
          public ICollection<Comment> Comments { get; set; }
      }
```
## <a name="data-annotations"></a>Anotaciones de datos

Código primero usa anotaciones desde el ensamblado System.ComponentModel.DataAnnotations como una manera de configurar primero las clases de código. Entre estas anotaciones son aquellos que proporcionan reglas como necesario, MinLength y MaxLength. Un número de aplicaciones de cliente de .NET también reconoce estas anotaciones, por ejemplo, ASP.NET MVC. Puede lograr ambos lado y servidor de validación en el cliente con estas anotaciones. Por ejemplo, puede forzar la propiedad Blog Title que es una propiedad necesaria.

``` csharp
    [Required]
    public string Title { get; set; }
```

Sin código adicional ni cambios de marcado en la aplicación, una aplicación existente de MVC realizará la validación del lado cliente, crear incluso dinámicamente un mensaje con los nombres de propiedad y anotación.

![figure01](~/ef6/media/figure01.png)

En la publicación back (método) de esta vista de creación, Entity Framework se usa para guardar el nuevo blog de la base de datos, pero se desencadene la validación del lado cliente de MVC antes de la aplicación llegue a ese código.

Validación del lado cliente pero no es infalible. Los usuarios pueden afectar a las características de su explorador o lo que es peor aún, un pirata informático podría utilizar algunos trucos para evitar las validaciones de interfaz de usuario. Pero Entity Framework también reconoce la anotación necesaria y validarlo.

Una manera sencilla de probar esto es deshabilitar la característica de validación del lado cliente de MVC. Puede hacerlo en el archivo web.config de la aplicación de MVC. La sección appSettings tiene una clave para ClientValidationEnabled. Si establece esta clave en false impedirá que la interfaz de usuario realizar validaciones.

``` xml
    <appSettings>
        <add key="ClientValidationEnabled"value="false"/>
        ...
    </appSettings>
```

Incluso con la validación del lado cliente deshabilitada, obtendrá la misma respuesta en la aplicación. Como se mostrará el mensaje de error "el campo título es obligatorio". Salvo que ahora será un resultado de validación del lado servidor. Entity Framework llevará a cabo la validación en la anotación necesaria (antes de que incluso lo molesta a la compilación y el comando de INSERCIÓN para enviar a la base de datos) y devolverá el error a MVC que se mostrará el mensaje.

## <a name="fluent-api"></a>API fluida

Puede usar código la API fluida del primero en lugar de las anotaciones de obtener el mismo cliente lado & servidor de validación de lado. En lugar de uso necesario, le mostraré este con una longitud máxima validación.

Se aplican configuraciones de la API Fluent como código en primer lugar es generar el modelo de las clases. Puede insertar las configuraciones invalidando el método OnModelCreating de la clase DbContext. Aquí es una configuración que especifica que la propiedad BloggerName no puede tener más de 10 caracteres.

``` csharp
    public class BlogContext : DbContext
      {
          public DbSet<Blog> Blogs { get; set; }
          public DbSet<Post> Posts { get; set; }
          public DbSet<Comment> Comments { get; set; }

          protected override void OnModelCreating(DbModelBuilder modelBuilder)
          {
              modelBuilder.Entity<Blog>().Property(p => p.BloggerName).HasMaxLength(10);
          }
        }
```

Errores de validación que se produce según las configuraciones de la API Fluent no automáticamente alcance la interfaz de usuario, pero puede capturará en el código y responder a ella en consecuencia.

Alguna excepción aquí es código de error en la clase de aplicación BlogController que captura ese error de validación cuando Entity Framework intenta guardar un blog con un BloggerName que supera el máximo de 10 caracteres de control.

``` csharp
    [HttpPost]
    public ActionResult Edit(int id, Blog blog)
    {
        try
        {
            db.Entry(blog).State = EntityState.Modified;
            db.SaveChanges();
            return RedirectToAction("Index");
        }
        catch(DbEntityValidationException ex)
        {
            var error = ex.EntityValidationErrors.First().ValidationErrors.First();
            this.ModelState.AddModelError(error.PropertyName, error.ErrorMessage);
            return View();
        }
    }
```

Automáticamente se no pasan la validación en la vista que es el motivo por el código adicional que utiliza ModelState.AddModelError se está usando. Esto garantiza que los detalles del error facilitan a la vista que, a continuación, usará el ValidationMessageFor Htmlhelper para mostrar el error.

``` csharp
    @Html.ValidationMessageFor(model => model.BloggerName)
```

## <a name="ivalidatableobject"></a>IValidatableObject

IValidatableObject es una interfaz que se encuentra en System.ComponentModel.DataAnnotations. Aunque no es parte de la API de Entity Framework, puede seguir aprovechando lo para la validación del lado servidor en las clases de Entity Framework. IValidatableObject proporciona un método Validate que llamará a Entity Framework durante SaveChanges, o bien puede llamar a usted mismo siempre que desee para validar las clases.

Configuraciones necesarias, como y MaxLength realizar validación en un único campo. En el método Validate puede tener incluso una lógica más compleja, por ejemplo, comparar dos campos.

En el ejemplo siguiente, la clase de Blog se amplió para implementar IValidatableObject y, a continuación, proporcionar una regla que no pueden coincidir con el título y BloggerName.

``` csharp
    public class Blog : IValidatableObject
     {
         public int Id { get; set; }
         [Required]
         public string Title { get; set; }
         public string BloggerName { get; set; }
         public DateTime DateCreated { get; set; }
         public virtual ICollection<Post> Posts { get; set; }

         public IEnumerable<ValidationResult> Validate(ValidationContext validationContext)
         {
             if (Title == BloggerName)
             {
                 yield return new ValidationResult
                  ("Blog Title cannot match Blogger Name", new[] { "Title", “BloggerName” });
             }
         }
     }
```

El constructor ValidationResult toma una cadena que representa el mensaje de error y una matriz de cadenas que representan los nombres de miembros que están asociados con la validación. Puesto que esta validación comprueba que el título y el BloggerName, se devuelven ambos nombres de propiedad.

A diferencia de la validación proporcionada por la API Fluent, este resultado de la validación se reconocerá la vista y el controlador de excepciones que usé anteriormente para agregar el error en la clase ModelState no es necesario. Dado que ambos nombres de propiedad establece en ValidationResult, el HtmlHelpers MVC muestra el mensaje de error para ambas propiedades de.

![figure02](~/ef6/media/figure02.png)

## <a name="dbcontextvalidateentity"></a>DbContext.ValidateEntity

DbContext tiene un método reemplazable denominado ValidateEntity. Cuando se llama a SaveChanges, Entity Framework llamará a este método para cada entidad en su caché cuyo estado no se modifica. Puede colocar lógica de validación directamente en aquí o, incluso, use este método para llamar a, por ejemplo, el método Blog.Validate agregado en la sección anterior.

Este es un ejemplo de una invalidación ValidateEntity que valida las entradas de nuevo para asegurarse de que el título de la publicación no se haya usado ya. En primer lugar comprueba si la entidad es una entrada de blog y que su estado se ha agregado. Si es así, busca en la base de datos para ver si ya hay una publicación con el mismo título. Si hay ya una publicación existente, se crea un nuevo DbEntityValidationResult.

DbEntityValidationResult aloja un DbEntityEntry y un ICollection de DbValidationErrors para una entidad única. Al principio de este método, se crea una instancia de un DbEntityValidationResult y, a continuación, se agregan los errores detectados en su colección ValidationErrors.

``` csharp
    protected override DbEntityValidationResult ValidateEntity (
        System.Data.Entity.Infrastructure.DbEntityEntry entityEntry,
        IDictionary\<object, object> items)
    {
        var result = new DbEntityValidationResult(entityEntry, new List<DbValidationError>());
        if (entityEntry.Entity is Post && entityEntry.State == EntityState.Added)
        {
            Post post = entityEntry.Entity as Post;
            //check for uniqueness of post title
            if (Posts.Where(p => p.Title == post.Title).Count() > 0)
            {
                result.ValidationErrors.Add(
                        new System.Data.Entity.Validation.DbValidationError("Title",
                        "Post title must be unique."));
            }
        }

        if (result.ValidationErrors.Count > 0)
        {
            return result;
        }
        else
        {
         return base.ValidateEntity(entityEntry, items);
        }
    }
```

## <a name="explicitly-triggering-validation"></a>Activar explícitamente la validación

Una llamada a SaveChanges desencadena todas las validaciones que se tratan en este artículo. Pero no debe depender de SaveChanges. Prefiere validar en otro lugar en la aplicación.

Se desencadenará DbContext.GetValidationErrors todas las validaciones, los definidos por las anotaciones o la API Fluent, la validación que se creó en IValidatableObject (por ejemplo, Blog.Validate) y las validaciones efectuadas en el DbContext.ValidateEntity método.

El código siguiente llamará GetValidationErrors en la instancia actual de una clase DbContext. ValidationErrors se agrupan por tipo de entidad en DbValidationRestuls. El código recorre en iteración en primer lugar a través de la DbValidationResults devuelto por el método y, a continuación, cada ValidationError dentro.

``` csharp
    foreach (var validationResults in db.GetValidationErrors())
        {
            foreach (var error in validationResults.ValidationErrors)
            {
                Debug.WriteLine(
                                  "Entity Property: {0}, Error {1}",
                                  error.PropertyName,
                                  error.ErrorMessage);
            }
        }
```

## <a name="other-considerations-when-using-validation"></a>Otras consideraciones al utilizar la validación

Estos son algunos otros puntos a tener en cuenta al utilizar la validación de Entity Framework:

-   La carga diferida está deshabilitada durante la validación.
-   EF validará las anotaciones de datos en las propiedades no asignados (propiedades que no están asignadas a una columna de la base de datos).
-   Después de que los cambios se detectan durante SaveChanges se realiza la validación. Si realiza cambios durante la validación es su responsabilidad para notificar el seguimiento de cambios.
-   DbUnexpectedValidationException se produce si se producen errores durante la validación.
-   Las facetas que Entity Framework se incluye en el modelo (longitud máxima, necesaria, etc.) producirá la validación, incluso si no hay anotaciones de datos en las clases o utilizado EF Designer para crear el modelo.
-   Reglas de prioridad:
    -   Llamadas a la API Fluent anular las anotaciones de datos correspondiente
-   Orden de ejecución:
    -   Validación de propiedades se produce antes de la validación de tipo
    -   Validación de tipo solo se produce si la propiedad validación es correcta
-   Si una propiedad es compleja también incluirá su validación:
    -   Validación de nivel de propiedad en las propiedades de tipo complejo
    -   Validación de nivel de tipo en el tipo complejo, incluida la validación de IValidatableObject en el tipo complejo

## <a name="summary"></a>Resumen

La API de validación en Entity Framework se reproduce perfectamente con validación del lado cliente en MVC, pero no tiene que depender de validación del lado cliente. Entity Framework se encargará de la validación en el servidor de DataAnnotations o configuraciones que se haya aplicado con la API Fluent de code first.

También ha visto un número de puntos de extensibilidad para personalizar el comportamiento si usa la interfaz IValidatableObject o pulse en el método DbContet.ValidateEntity. Y estos últimos dos medias de validación están disponibles a través de la clase DbContext, independientemente de si usa el flujo de trabajo de Code First, Model First o primera base de datos para describir el modelo conceptual.
